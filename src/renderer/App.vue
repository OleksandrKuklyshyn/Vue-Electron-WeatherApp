<template>
  <div id="app">
    <div class="weather-header">
      <p>Weather App</p>
    </div>

    <div class="weather-input">
      <input type="text" v-model="query" placeholder="Enter yours city" @keypress="showWeather">
      <button class="button" :disabled="!query.length" @click="showWeather">Check</button>
    </div>

    <div v-if="error" class="weather-error">
      There is no such city in the database
    </div>

    <div v-if="city.length" class="weather-result">
      <h1>{{city}}, {{country}}</h1>
      <p><em>{{weatherDescription}}</em></p>

      <div class="weather-result__main">
        <img :src="icon" alt="Weather icon">
        <div class="weather-result__temp">
          {{temp}}&deg;C
        </div>
      </div>

      <div class="weather-result__details">
        <p>Min: {{tempMin}}&deg;C</p>
        <p>Max: {{tempMax}}&deg;C</p>
        <p>Humidity: {{humidity}}%</p>
      </div>

    </div>
    
  </div>
</template>

<script>
  import LandingPage from '@/components/LandingPage'

  export default {
    name: 'weatherapp',
    data() {
    return {
      query: '',
      error: false,
      city: '',
      country: '',
      weatherDescription: '',
      temp: null,
      tempMin: null,
      tempMax: null,
      humidity: null,
      icon: '',
    }},
    components: {
      LandingPage
    },
    methods: {
      showWeather() {
        this.$http
        .get(`/weather?q=${this.query}&units=metric&&appid=a168f6f45b1bd955ce47287ae5c6bac4`)//`/weather?q=${this.query}&units=metric&&appid=a168f6f45b1bd955ce47287ae5c6bac4`
        .then(response => {
          this.city = response.data.name;
          this.country = response.data.sys.country;
        this.weatherDescription = response.data.weather[0].description;
        this.temp = response.data.main.temp;
        this.tempMin = response.data.main.temp_min;
        this.tempMax = response.data.main.temp_max;
        this.humidity = response.data.main.humidity;
        this.icon = `http://openweathermap.org/img/w/${
          response.data.weather[0].icon
          }.png`;
          this.error = false;
        })
        .catch(()=>{
          this.error = true;
          this.city = '';
        });
      }
    }
  }
</script>

<style>
  
html,
body,
#app {
  height: 100%;
}
#app {
  font-family: Arial, Helvetica, sans-serif;

  font-size: 16px;
  padding: 10px;
  background: rgb(212, 228, 239);
  background: -moz-radial-gradient(
    center,
    ellipse cover,
    rgba(212, 228, 239, 1) 0%,
    rgba(134, 174, 204, 1) 100%
  );
  background: -webkit-radial-gradient(
    center,
    ellipse cover,
    rgba(212, 228, 239, 1) 0%,
    rgba(134, 174, 204, 1) 100%
  );
  background: radial-gradient(
    ellipse at center,
    rgba(212, 228, 239, 1) 0%,
    rgba(134, 174, 204, 1) 100%
  );
  
}

.weather-header{
  font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-size: 35px;
  margin-bottom: 30px; 
  font-weight: 700;
  
  text-align: center;
}

.weather-input{
  display: flex;
  width: 80%;
  margin: 0 auto;

}
input {
  width: 75%;
font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
padding-left: 20px;
font-size: 20px;
  outline: none;
  
  border-radius: 8px;
  height: 1.8em;
}
button {
  width: 20%;
 margin-left: 30px;
 font-size: 18px;
 border-radius: 8px;
}
.weather-result{
 
  margin-top: 40px;
  text-align: center;

}
.weather-result__main{
  display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 5px;
    font-size: 1.3rem;
    font-weight: bold;
}
.weather-result__details{
  display: flex;
  align-items: center;
    justify-content: space-around;
    padding-top: 10px;
}

</style>
