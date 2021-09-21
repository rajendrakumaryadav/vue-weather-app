<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input v-model="query"
               class="search-bar"
               placeholder="Search..."
               type="text"
               @keypress="fetch_Weather">

      </div>
      <div v-if="typeof weather.main != 'undefined'" class="weather-wrapper">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}&deg;C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: "93ebccc50f160b72ab1a2367fc63ecdd",
      base_url: "https://api.openweathermap.org/data/2.5/weather?q=",
      query: "",
      weather: {}
    }
  },
  methods: {
    fetch_Weather(e) {
      if (e.key === "Enter") {
        fetch(this.base_url + this.query + "&units=metric&APPID=" + this.api_key)
            .then(res => {
              console.log(res)
              return res.json()
            }).then(this.setResults)
            .catch(e => {
              console.log("Error " + e)
            }).finally(() => {
          console.log("At the end!")
        })
      }
    },
    setResults(results) {
      this.weather = results;
    }
  }
}
</script>

<style lang="css">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

#app {
  background-size: cover;
  background: #FAC0FA bottom;
  transition: 0.4s;
}

main {
  height: 100vh;
  padding: 25px;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
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
  outline: none;
  background: rgba(255, 255, 255, 0.15) none;
  border-radius: 0 16px 0 16px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
  transition: 0.5s;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFFFFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.75);
}

.weather-box .weather {
  color: #FFFFFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
