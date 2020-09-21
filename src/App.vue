<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search ..."
          v-model="query"
          @keypress="getWeather"
        />
      </div>
      <div class="weather-container" v-if="weather.location">
        <div class="location-box">
          <div class="location">{{weather.location}}</div>
          <div class="date">{{weather.date}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">
            <img class="weather_icon" :src="weather.weather_icon_url" alt />
            {{weather.tempF}}°F
            <span class="celcius">{{weather.tempC}}°C</span>
            <div class="weather">{{weather.description}}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "3301b177e6a9f6fbcdd32ac6a4f1e3f3",
      base_url: "https://api.openweathermap.org/data/2.5/",
      weather: {
        tempF: "",
        tempC: "",
        description: "",
        location: "",
        date: "",
        weather_icon_url: "",
      },
      query: "",
    };
  },
  methods: {
    getWeather(e) {
      if (e.code == "Enter") {
        fetch(`${this.base_url}weather?q=${this.query}&appid=${this.api_key}`)
          .then((res) => {
            return res.json();
          })
          .then(this.showResults);
        this.query = "";
      }
    },
    showResults(results) {
      this.weather.tempC = (results.main.temp - 273.15).toFixed(1);
      this.weather.tempF = ((this.weather.tempC * 9) / 5 + 32).toFixed(1);
      this.weather.description = results.weather[0].description;
      this.weather.weather_icon_url = `https://openweathermap.org/img/wn/${results.weather[0].icon}@2x.png`;
      this.weather.date = new Date().toLocaleString();
      this.weather.location = results.name + " , " + results.sys.country;
    },
  },
};
</script>

<style>
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  text-align: center;
}
body {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
}
#app {
  background-image: url("https://i.pinimg.com/originals/a8/7b/66/a87b66b8337ed8239efa234826febf6e.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 2.5rem;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 3rem;
}
.search-bar {
  display: block;
  width: 100%;
  padding: 1rem;

  color: #0842f1;
  font-size: larger;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.6s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.75);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  font-size: x-large;
  color: rgba(255, 255, 255, 0.85);
}
.location-box .date {
  font-size: large;
  color: rgba(255, 255, 255, 0.5);
  font-style: italic;
}
.weather-box .temp {
  padding: 1rem 1rem;
  font-size: xx-large;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 6px;
  transition: 0.6s;

  width: 25%;
  margin: auto;
  margin-top: 1rem;
}
.weather-box .weather {
  color: rgba(0, 0, 0, 0.75);
  padding: 1rem;
  font-size: large;
}
.celcius {
  font-size: small;
  display: block;
}
.weather_icon {
  display: block;
  margin: auto;
}
</style>
