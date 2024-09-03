<template>
  <div class="weather-app">
    <h1>Weather App</h1>
    <input v-model="city" @keyup.enter="fetchWeather" placeholder="Enter city name" />
    <button @click="fetchWeather">Get Weather</button>
    <div v-if="weather" class="weather-info">
      <h2>Weather in {{ weather.name }}</h2>
      <p class="temperature">Temperature: {{ (weather.main.temp - 273.15).toFixed(2) }} °C</p>
      <p>Weather: {{ weather.weather[0].description }}</p>
      <p>Humidity: {{ weather.main.humidity }}%</p>
      <p>Wind Speed: {{ weather.wind.speed }} m/s</p>
    </div>
    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      weather: null,
      error: null
    }
  },
  methods: {
    async fetchWeather() {
      if (!this.city) return;
      const apiKey = '4f8024851bdc433835951f8ad3e2b582'; // Replace with your OpenWeatherMap API key
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}`;
      try {
        const response = await axios.get(url);
        this.weather = response.data;
        this.error = null;
      } catch (err) {
        this.error = 'City not found or API error.';
        this.weather = null;
      }
    }
  }
}
</script>

<style scoped>
.weather-app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background: linear-gradient(135deg, #6dd5ed, #2193b0);
  color: #ffffff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  padding: 20px;
  text-align: center;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  color: #ffffff;
}

input {
  width: 100%;
  max-width: 350px;
  padding: 12px;
  margin: 10px 0;
  border: 2px solid #ddd;
  border-radius: 25px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.3s ease;
}

input:focus {
  border-color: #007bff;
}

button {
  background-color: #ff7f50;
  border: none;
  color: white;
  padding: 10px 20px;
  font-size: 1rem;
  border-radius: 25px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
  margin-top: 10px;
}

button:hover {
  background-color: #e05648;
  transform: translateY(-2px);
}

.weather-info {
  background-color: rgba(255, 255, 255, 0.2);
  padding: 20px;
  border-radius: 10px;
  margin-top: 20px;
  backdrop-filter: blur(10px);
}

.weather-info h2 {
  margin-bottom: 10px;
  font-size: 1.8rem;
  color: #ffffff;
}

.temperature {
  font-size: 1.5rem;
  font-weight: bold;
  margin-top: 10px;
  color: #f8f9fa;
}

p {
  margin: 5px 0;
  font-size: 1.1rem;
  color: #f1f1f1;
}

.error {
  color: #ff4d4f;
  font-size: 1.2rem;
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ff4d4f;
  border-radius: 5px;
  background-color: rgba(255, 77, 79, 0.1);
}
</style>
