<template>
  <div id="app" :class="weather.main && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="search"
          v-model="querry"
          name="searchBar"
          class="search-bar"
          placeholder="Search..."
          @keypress.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <!-- <div class="date">{{ date.toDateString() }}</div> -->
          <div class="date">{{ date.toDateString() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}℃</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
// imports
import { ref } from "vue";

// main setup
let apiKey = ref("dcc0d8b2dcee9da2e8e49e2116503812");
let baseUrl = ref('https://api.openweathermap.org/data/2.5/')
// let baseUrl = ref("https://pro.openweathermap.org/data/2.5/forecast/hourly");
let querry = ref("");
let weather = ref({
  name: 'Islamabad',
  sys: {
    country: 'PK'
  }
});
let date = ref(new Date())

const setResults = (res) => {
  weather.value = res
  date.value = new Date()
  debugger
  console.log(weather)
  console.log(date)
}

const fetchWeather = (e) => {
  fetch(`${baseUrl.value}weather?q=${querry.value}&units=metric&appid=${apiKey.value}`)
    .then((res) => {
      return res.json();
    })
    .then((res) => setResults(res));
};

</script>

<style scoped>
#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  background: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  outline: none;
  border-radius: 16px 0 16px 0;
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  font: 32px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  font: 20px;
  font-weight: 300;
  color: #fff;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  padding: 10px 25px;
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
