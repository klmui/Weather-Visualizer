<template>
  <!-- This is a colon shorthand binding the class -->
  <div id="app" :class="isWeatherWarm() ? 'warm' : ''">
    <main>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          />
          <div style="margin-top: 5px; text-align: center">
            <p style="font-size: 0.6em">
              Enter the "city, state" e.g. "madison, wisconsin"
            </p>
          </div>
        </div>
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div
            class="date"
            style="
              display: flex;
              align-items: center;
              justify-content: space-around;
            "
          >
            <span style="">{{ dateBuilder() }}</span>
            <img id="icon" :src="getIconSrc()" />
          </div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            <div class="feels-like">
              Feels like {{ Math.round(weather.main.feels_like) }}°C
            </div>
            <div>
              <canvas
                style="margin-top: 15px"
                id="skycon"
                width="90"
                height="90"
              ></canvas>
            </div>
          </div>
        </div>
      </div>
      <div class="weather-wrap" v-else>
        <h2 style="color: #fff; margin-bottom: 10px">Search any city</h2>
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          />
          <div style="margin-top: 5px; text-align: center">
            <p style="font-size: 0.6em">
              Enter the "city, state" e.g. "madison, wisconsin"
            </p>
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
      api_key: "620cb8cc343201300b251cb1dcb52d5e",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  mounted: function () {
    // On page load
    if (!navigator.geolocation) {
      return false;
    }
    navigator.geolocation.getCurrentPosition((position) => {
      // Fetch weather
      let long = position.coords.longitude;
      let lat = position.coords.latitude;
      fetch(
        `${this.url_base}weather?lat=${lat}&lon=${long}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then((res) => {
          this.setResults(res);
        });
    });
  },
  methods: {
    getIconSrc() {
      if (!this.weather.weather) {
        return "";
      }
      return `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`;
    },

    isWeatherWarm() {
      if (!this.weather.main) {
        return true;
      }
      if (this.weather.main.temp > 0) {
        return true;
      }
      return false;
    },

    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query.trim()}&units=metric&APPID=${
            this.api_key
          }`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
  z-index: -1;
  background: linear-gradient(rgba(47, 150, 163), rgba(48, 62, 143));
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  /* background-size: cover; */
  background-repeat: no-repeat;
  background-position: center;
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
  ) !important;
}

.search-box {
  width: 100%;
  margin-bottom: 25px;
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
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 016px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  text-align: center;
  margin-bottom: 5px;

  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
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
  margin: 20px 0px 0px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

#icon {
  width: 35px;
  height: 35px;
}

.weather-wrap {
  position: absolute;
  left: 50%;
  margin-right: -50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.weather-box .feels-like {
  color: #fff;
}
</style>
