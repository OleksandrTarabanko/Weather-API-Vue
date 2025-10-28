<script>
export default {
  data() {
    return {
      city: "Hamburg",
      country: "Germany",
      currentTime: null,
      location: null,
      weatherData: null,
      temperature: null,
      humidity: null,

    }
  },
  methods: {
    async searchWeather() {
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

      this.temperature = temperatureData[searchElementIndex] + "°C"
      this.humidity = humidityData[searchElementIndex] + "%"
      this.windSpeed = windSpeedData[searchElementIndex] + " km/h"
    },

  },
  mounted() {
    this.currentTime = new Date()
    console.log("current time is:", this.currentTime)
  },
}
</script>

<template>
  <input v-model.trim="city" type="text" placeholder="Your City">
  <hr>
  <input v-model.trim="country" type="text" placeholder="Your Country">
  <hr>
  <button @click="searchWeather(city, country)">Search</button>
  <div>
    <h2>You're lookig for weather in {{ city }}, {{ country }}</h2>
    <p>{{ temperature }}</p>
    <p>{{ humidity }}</p>
    <p>{{ windSpeed }}</p>
  </div>
</template>

<style scoped></style>
