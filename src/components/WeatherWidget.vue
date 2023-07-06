<template>
  <div class="weather-widget" :class="{ loading: !weatherData }">
    <h2 class="widget-title">Widget Cuaca</h2>
    <div class="location-input">
      <label for="location">Enter Location:</label>
      <input type="text" id="location" v-model="location" />
      <button @click="fetchWeatherData">Get Weather</button>
    </div>
    <div v-if="weatherData" class="weather-data" v-show="weatherData">
      <p class="location">Location: {{ weatherData.name }}</p>
      <p v-if="weatherData.main" class="temperature">
        Celcius: {{ Math.round(weatherData.main.temp - 273.15) }}°C
      </p>
      <p v-if="weatherData.weather" class="description">
        Description: {{ weatherData.weather[0].description }}
      </p>
    </div>
    <p v-else class="loading">Loading weather data... <span class="loading-icon"></span></p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: '',
      weatherData: null
    };
  },
  methods: {
    async fetchWeatherData() {
      try {
        const apiKey = 'b7bfca7b27a3485144fea086c50d09dc';
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${apiKey}`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        this.weatherData = data;
      } catch (error) {
        console.error('Error fetching weather data:', error);
      }
    }
  }
};
</script>

<style scoped>
.weather-widget {
  border: 1px solid #ffffff;
  padding: 20px;
  margin-bottom: 20px;
  text-align: center;
  background-color: #fff;
}

.weather-widget.loading {
  background-color: #ffffff;
}

.widget-title {
  margin-top: 0;
  color: #333;
}

.location-input {
  margin-bottom: 10px;
}

.weather-data {
  margin-top: 10px;
  transition: opacity 0.3s ease;
}

.weather-data.fade-enter-active,
.weather-data.fade-leave-active {
  transition: opacity 0.3s ease;
}

.weather-data.fade-enter,
.weather-data.fade-leave-to {
  opacity: 0;
}

.location {
  font-size: 18px;
  font-weight: bold;
}

.temperature {
  font-size: 24px;
  color: #ff5722;
}

.description {
  font-size: 16px;
}

button:hover {
  background-color: #ff5722;
  color: #fff;
}

.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.loading-icon {
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: #333;
  animation: loadingAnimation 1s linear infinite;
}

@keyframes loadingAnimation {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
</style>
