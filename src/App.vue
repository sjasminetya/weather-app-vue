<template>
  <div id="app" :class="typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box" v-if="typeof weather.main === 'undefined' ">
        <input type="text" class="search-bar" placeholder="Search ..." v-model="query" @keypress="fetchWeather">
      </div>
      
      <div v-else>
        <div class="search-box">
          <input type="text" class="search-bar" placeholder="Search ..." v-model="query" @keypress="fetchWeather">
        </div>

        <div class="weather-wrap">
          <div class="location-box">
            <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
            <div class="date">{{dateBuilder()}}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{Math.round(weather.main.temp)}}⁰c</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  title: 'Weather App',
  data () {
    return {
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        axios.get(`${process.env.VUE_APP_URLBASE}weather?q=${this.query}&units=metric&APPID=${process.env.VUE_APP_APIKEY}`)
        .then(res => {
          this.weather = res.data
          console.log(this.weather)
        })
      }
    },
    dateBuilder () {
      let d = new Date()
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
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
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/winter.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

main .search-box {
  width: 100%;
  margin-bottom: 30px;
}

main .search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.55);
  background-color: rgba(255, 255, 255, 0.40);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

main .search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

main .location-box .location,
main .location-box .date {
  color: #FFFFFF;
  font-weight: bold;
  font-size: 32px;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.45);
}

main .location-box .date {
  font-weight: 300;
  font-size: 20px;
  font-style: italic;
  text-shadow: none;
}

main .weather-box {
  text-align: center;
}

main .weather-box .temp {
  display: inline-block;
  padding: 20px 25px;
  color: #FFFFFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

main .weather-box .weather {
  color: #FFFFFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
