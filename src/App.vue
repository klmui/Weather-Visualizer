<template>
  <!-- This is a query for binding the class -->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 60 ? 
    'warm' : ''">
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
          <!-- Used two-way binding above using v-model -->
          <!-- {{ query }} -->
        </div>
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
          <div class="time"> {{ weather.timezone }} </div>
          
          
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°f</div>
            <!-- <div class="weather">{{ weather.weather[0].main }}</div> -->
            <div class="icon">icon</div>
          </div>
        </div>
      </div>
      <div class="weather-wrap" v-else-if="!locationFound">
        <div class="search-box">
          <input 
            type="text" 
            class="search-bar" 
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          />
          <!-- Used two-way binding above using v-model -->
          <!-- {{ query }} -->
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
      api_key: '620cb8cc343201300b251cb1dcb52d5e',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      locationFound: navigator.geolocation
    }
  }, 
  // On page load
  mounted:function() {
    let long;
    let lat;

    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        // Fetch weather
        long = position.coords.longitude;
        lat = position.coords.latitude;
        fetch(`${this.url_base}weather?lat=${lat}&lon=${long}&units=imperial&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults);
      });
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        // fetch() is from JS
        fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
  z-index: -1;
  background: linear-gradient(rgba(47,150,163), rgba(48,62,143));
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  /* background-size: cover; */
  background-repeat: no-repeat;
  background-position: center;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg')
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75)) !important;

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
  color: #FFF;
  text-align: center;
  margin-bottom: 5px;

  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-wrap {
  position: absolute;
  left: 50%;
  margin-right: -50%;
  top: 50%;
  transform: translate(-50%, -50%)
}
</style>
