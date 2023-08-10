<template>
  <SearchBox
    v-model="query"
    @fetchWeather="fetchWeather"
    :value="query"
    :api_key="api_key"
    :url_base="url_base"
    @enter-pressed="fetchWeatherForQuery"
    @selected-suggestion="fetchWeatherForQuery"
  />
  <div
    v-if="weather && weather.location && weather.location.name"
    class="weather-wrap"
  >
    <div class="location-box">
      <div class="location">
        {{ `${weather.location.name}, ${weather.location.country}` }}
      </div>
      <div class="date">{{ dateBuilder() }}</div>
    </div>
    <div class="weather-box">
      <div class="temp">{{ Math.round(weather.current.temp_c) }}°C</div>
      <div class="weather">{{ weather.current.condition.text }}</div>
    </div>
  </div>
</template>
  
  <script>
import SearchBox from "./SearchBox.vue";

export default {
  name: "WeatherDetails",
  components: {
    SearchBox,
  },
  data() {
    return {
      api_key: "d9e582abd07d4d9ab7f130126230708",
      url_base: "http://api.weatherapi.com/v1",
      query: "",
      weather: null,
    };
  },
  methods: {
    async fetchWeather(e, query) {
      if (e.key === "Enter") {
        try {
          const response = await fetch(
            `${this.url_base}/current.json?key=${this.api_key}&q=${query}&aqi=no`
          );

          if (!response.ok) {
            return;
          }

          const data = await response.json();
          this.setResults(data);
          this.showAutocomplete = false;
        } catch (error) {
          console.error("Error fetching weather:", error);
          this.weather = null;
        }
      }
    },
    async fetchWeatherForQuery(query) {
      this.fetchWeather({ key: "Enter" }, query);
      this.query = query;
    },
    setResults(results) {
      this.weather = results;
      this.weatherClass();
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
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    async fetchWeatherForUserLocation() {
      try {
        const ipResponse = await fetch(`https://api-bdc.net/data/client-ip`);
        const ipData = await ipResponse.json();

        
        const locationResponse = await fetch(
          `${this.url_base}/ip.json?key=${this.api_key}&q=${ipData.ipString}`
        );
        const locationData = await locationResponse.json();

        
        const weatherResponse = await fetch(
          `${this.url_base}/current.json?key=${this.api_key}&q=${locationData.country_name}&aqi=no`
        );
        const weatherData = await weatherResponse.json();

        
        this.weather = weatherData;
        this.weatherClass();
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
    weatherClass() {
      if (this.weather && this.weather.current.temp_c > 16 && this.weather.current.is_day == 1) {
        this.$emit("set-background", "warm-day"); 
      } else if (this.weather && this.weather.current.temp_c > 16 && this.weather.current.is_day == 0){
        this.$emit("set-background", "warm-night"); 
      } else if (this.weather && this.weather.current.temp_c < 16 && this.weather.current.is_day == 0){
        this.$emit("set-background", "cold-night"); 
      } else if (this.weather && this.weather.current.temp_c < 16 && this.weather.current.is_day == 1){
        this.$emit("set-background", ""); 
      }
    },
  },
  created() {
    this.fetchWeatherForUserLocation(); // Fetch weather for the user's location when the component is created
  },
};
</script>
  
  <style>
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("/src/assets/cold-bc.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm-day {
  background-image: url("/src/assets/warm-bc.jpg");
}

#app.warm-night {
  background-image: url("/src/assets/warm-night.jpg");
}

#app.cold-night {
  background-image: url("/src/assets/cold-night.jpg");
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
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
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
  font-weight: 500;
  text-align: center;
  font-style: italic;
}
</style>