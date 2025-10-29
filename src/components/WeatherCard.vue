<script>

export default {
    props: {
        id: Number,
        city: String,
        country: String,
        onDelete: Function,
    },

    data() {
        return {
            location: null,
            temperature: null,
            humidity: null,
            windSpeed: null,
            currentTime: null,

        }
    },

    async mounted() {
        this.currentTime = new Date()


        const coordinatesURL = `https://nominatim.openstreetmap.org/search?city=${this.city}&country=${this.country}&format=json`
        const coordinatesResponse = await fetch(coordinatesURL);
        const coordinatesData = (await coordinatesResponse.json())[0];
        const location = coordinatesData
        const weatherURL = `https://api.open-meteo.com/v1/forecast?latitude=${location.lat}&longitude=${location.lon}&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m`;
        const weatherResponse = await fetch(weatherURL);
        const weatherDataServer = await weatherResponse.json();
        const weatherData = weatherDataServer.hourly
        console.log("location is", location)
        console.log("weather:", weatherData)

        const {
            time,
            temperature_2m: temperatureData,
            relative_humidity_2m: humidityData,
            wind_speed_10m: windSpeedData
        } = weatherData;

        const searchElementIndex = time.map(el => new Date(el)).findIndex(el => el > this.currentTime)



        this.location = location.display_name
        this.temperature = "Temperature is " + temperatureData[searchElementIndex] + "°C"
        this.humidity = "Humidity is " + humidityData[searchElementIndex] + "%"
        this.windSpeed = "Wind speed is " + windSpeedData[searchElementIndex] + " km/h"

    }
}

</script>

<template>
    <div v-if="temperature" class="main-div">
        <div class="element-div">
            <h2>You're lookig for weather in {{ location }}</h2>
            <p>{{ temperature }}</p>
            <p>{{ humidity }}</p>
            <p>{{ windSpeed }}</p>
            <button @click="onDelete(this.id)" class="delete-btn">Delete</button>
        </div>
    </div>
    <div v-else>
        <h1>LOADING</h1>
    </div>
</template>

<style scoped>
.main-div {
    margin: 15px;
    display: flex;
    text-align: start;
    flex-direction: column;
    gap: 10px;
}

.element-div {
    display: flex;
    flex-direction: column;
    max-width: 400px;
    padding: 10px;
    border: 3px solid black;
}

.delete-btn {
    margin-left: auto;
    border: 1px solid black;
}

.delete-btn:hover {
    border-color: red;
}
</style>