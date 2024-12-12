<template>
  <div v-bind:id="dynamicId">
    <main>
      <!-- Search Box -->
      <div class="search-box">
        <div class="m">
          <input
            type="text"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          />
        </div>
      </div>

      <!-- Weather Information -->
      <div class="weather-wrap" v-if="weather.main">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      api_key: "88e5eeaea4deab13fd52f9128bcafc23",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then((res) => res.json())
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      const d = new Date();
      const months = [
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
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day}, ${date} ${month} ${year}`;
    },
  },
  computed: {
    dynamicId() {
      if (this.weather.weather && this.weather.weather.length > 0) {
        const icon = this.weather.weather[0].icon;
        if (icon.includes("n")) return "app-warm";
        if (["01d", "02d"].includes(icon)) return "clear-sky";
        if (["10d", "11d", "09d"].includes(icon)) return "rain";
        if (["03d", "04d"].includes(icon)) return "cloudy";
      }
      return "snow";
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
  font-family: "Montserrat", sans-serif;
}

#rain {
  background-image: url("./assets/rain.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#clear-sky {
  background-image: url("./assets/clear-sky.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#snow {
  background-image: url("./assets/snow.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app-warm {
  background-image: url("./assets/warm-bg.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#cloudy {
  background-image: url("./assets/cloudy.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
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
  border: none;
  appearance: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  transition: 0.2s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  text-align: center;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 10px 10px 8px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  font-size: 48px;
  color: #fff;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
