<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <h1 class="title">Weather App</h1>
      <div class="city-box">
        <button class="city-bar" v-for="city in cities" :key="city" @click="fetchCity(city)">{{ city }}</button>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather-main">{{weather.weather[0].main}}</div>
        </div>
        <div class="weather-box">
          <div class="weather"> Atmospheric Pressure: {{weather.main.pressure}} hPa</div>
          <div class="weather"> Humidity: {{weather.main.humidity}} %</div>
          <div class="weather"> Wind speed: {{weather.wind.speed}} meter/sec</div>
          <div class="weather"> Wind direction: {{weather.wind.deg}} °</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: 'a39698134cfd2a34cbe4a458399c6e3d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      cities: ['Kyiv', 'London', 'New York']
    }
  },
  methods: {
    fetchCity(cityName) {
      fetch(`${this.url_base}weather?q=${cityName}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults) 
      },
    setResults (results) {
      this.weather = results;
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  body {
    font-family: 'monsterrat', sans-serif;
  }

  #app {
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0,4s;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
  }

  .title {
    margin: 25px;
    text-align: center;
  }

  .city-box {
    width: 70%;
    margin: auto;
    margin-bottom: 30px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .city-box .city-bar {
    width: 30%;
    padding: 65px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 16px rgba(0,0,0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .city-box .city-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
  }

  .weather-wrap {
    width: 70%;
    margin: auto;
    margin-top: 10vh;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #FFF;
    font-size: 172px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 0px 0px 30px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather {
    color: #FFF;
    font-size: 2em;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
    text-align: end;
    margin: auto;
  }

  .weather-box .weather-main {
    font-size: 68px;
    color: #FFF;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
    text-align: center;
  }

</style>