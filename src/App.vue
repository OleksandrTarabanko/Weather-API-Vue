<script>
export default {
  data() {
    return {
      city: null,
      country: null,
      currentTime: null,
      location: null,
      weatherData: null,
      counter: 0,
      result: []

    }
  },
  methods: {
    async searchWeather() {
      if (this.city && this.country) {

        const coordinatesURL = `https://nominatim.openstreetmap.org/search?city=${this.city}&country=${this.country}&format=json`
        const coordinatesResponse = await fetch(coordinatesURL);
        const coordinatesData = (await coordinatesResponse.json())[0];
        this.location = coordinatesData
        const weatherURL = `https://api.open-meteo.com/v1/forecast?latitude=${this.location.lat}&longitude=${this.location.lon}&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m`;
        const weatherResponse = await fetch(weatherURL);
        const weatherDataServer = await weatherResponse.json();
        this.weatherData = weatherDataServer.hourly
        console.log("location is", this.location)
        console.log("weather:", this.weatherData)

        const {
          time,
          temperature_2m: temperatureData,
          relative_humidity_2m: humidityData,
          wind_speed_10m: windSpeedData
        } = this.weatherData;

        const searchElementIndex = time.map(el => new Date(el)).findIndex(el => el > this.currentTime)

        this.temperature = "Temperature is " + temperatureData[searchElementIndex] + "°C"
        this.humidity = "Humidity is " + humidityData[searchElementIndex] + "%"
        this.windSpeed = "The wind speed is " + windSpeedData[searchElementIndex] + " km/h"


        this.result.push({
          id: this.counter,
          location: this.location.display_name,
          temperature: "Temperature is " + temperatureData[searchElementIndex] + "°C",
          humidity: "Humidity is " + humidityData[searchElementIndex] + "%",
          windSpeed: "Wind speed is " + windSpeedData[searchElementIndex] + " km/h"
        })

        this.counter++
      } else {
        alert("ERROR: Fill every input")
      }

    },

    deleteWeatherCard(id) {
      this.result = this.result.filter(el => el.id !== id)
    }
  },
  mounted() {
    this.currentTime = new Date()
  },
}
</script>

<template>
  <input v-model.trim="city" type="text" placeholder="Your City">
  <hr>
  <input v-model.trim="country" type="text" placeholder="Your Country">
  <hr>
  <button @click="searchWeather(city, country)">Search</button>
  <div class="main-div">
    <div v-for="el in result" :key="el.id" class="element-div">
      <h2>You're lookig for weather in {{ el.location }}</h2>
      <p>{{ el.temperature }}</p>
      <p>{{ el.humidity }}</p>
      <p>{{ el.windSpeed }}</p>
      <button @click="deleteWeatherCard(el.id)" class="delete-btn">Delete</button>
    </div>
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
