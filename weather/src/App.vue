<script>
export default {
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes('Sunny')) {
        return 'sunny';
      } else if (this.description.includes('Overcast')) {
        return 'overcast'
      } else if (this.description.includes('Partly cloudy')) {
        return 'partly-cloudy'
      }else{
        return '';
      }
    }
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(`https://api.weatherapi.com/v1/current.json?key=cf417869ed68418d812134541240206&q=${this.searchQuery}`)
        .then(res => res.json())
        .then(data => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .cath(error => {
          this.loading = false;
          this.error = true;
          console.log(error)
        });
    },
    resetSearchQuery() {

    }
  }
}
</script>

<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input type="text" name="" id="" class="weather-form__input" v-model="searchQuery" placeholder="Enter city"
          @keyup.enter="weatherSearch">
        <button class="weather-form__bth" @click="weatherSearch">Search</button>
      </div>
      <div class="card weather-load" v-if="loading">Loading...</div>
      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">
        <div class="card" v-if="error">Error</div>
        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="./assets/basic-bg.jpg" alt="App Background">
        <img class="weather-bg__img overcast" src="./assets/cloudy-bg.jpg" alt="Cloudy Background">
        <img class="weather-bg__img partly-cloudy" src="./assets/partly-cloudy.jpg" alt="Partly Cloudy">
        <img class="weather-bg__img sunny" src="./assets/sunny-bg.jpg" alt="Sunny">
      </div>
    </div>
  </div>
</template>

<style></style>
