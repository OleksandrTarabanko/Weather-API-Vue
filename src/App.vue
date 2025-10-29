<script>
import SearchButton from './components/SearchButton.vue';
import WeatherCard from './components/WeatherCard.vue';

export default {
  components: {
    SearchButton,
    WeatherCard,
  },
  data() {
    return {
      city: null,
      country: null,
      currentTime: null,
      counter: 0,
      cards: []
    }
  },
  methods: {
    searchWeather() {
      if (this.city && this.country) {

        this.cards.push({
          id: this.counter,
          city: this.city,
          country: this.country,
        })
        this.counter++
      } else {
        alert("ERROR: Fill every input")
      }
    },

    deleteCard(id) {
      console.log(this.cards)
      this.cards = this.cards.filter(el => el.id !== id)
      console.log(this.cards)
    }
  },
}
</script>

<template>
  <input v-model.trim="city" type="text" placeholder="Your City">
  <hr>
  <input v-model.trim="country" type="text" placeholder="Your Country">
  <hr>

  <!-- <button @click="searchWeather">Search</button> -->
  <SearchButton text="Search" :action="searchWeather" />
  <!-- <div class="main-div">
    <div v-for="el in result" :key="el.id" class="element-div">
      <h2>You're lookig for weather in {{ el.location }}</h2>
      <p>{{ el.temperature }}</p>
      <p>{{ el.humidity }}</p>
      <p>{{ el.windSpeed }}</p>
      <button @click="deleteWeatherCard(el.id)" class="delete-btn">Delete</button>
    </div>
  </div> -->

  <WeatherCard v-for="card in cards" :on-delete="deleteCard" :city="card.city" :country="card.country" :id="card.id" />
</template>

<style scoped></style>
