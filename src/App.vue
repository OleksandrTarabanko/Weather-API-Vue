<script>
export default {
  data() {
    return {
      city: null,
      country: null,
      currentTime: null,
      // coordinatesURL: "https://nominatim.openstreetmap.org/search?city=&country=&format=json",
      // weatherURL: "https://api.open-meteo.com/v1/forecast?latitude=&longitude=&hourly=temperature_2m,relative_humidity_2m,weather_code,wind_speed_10m"
    }
  },
  methods: {
    searchWeather() {

    },

  },
  computed: {

  }
}
</script>

<template>
  <input v-model.trim="city" type="text" placeholder="Your City">
  <input v-model.trim="country" type="text" placeholder="Your Country">

  <button @click="searchWeather">Search</button>
</template>

<style scoped></style>
