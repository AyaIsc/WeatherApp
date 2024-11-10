<script setup>
import { ref } from 'vue';
import axios from 'axios';

const searchQuery = ref('');
const weatherData = ref(null);

const getSearchResults = async () => {
  if (searchQuery.value.trim() === '') {
    weatherData.value = null;
    return;
  }

  try {
    const response = await axios.get(`https://api.weatherapi.com/v1/current.json`, {
      params: {
        key: 'b031c9d4d49e4fa7971222807241011', 
        q: searchQuery.value,
        aqi: 'no', 
      }
    });
    
    weatherData.value = response.data;
  } catch (error) {
    console.error('Error : could not find the weathers data !', error);
    weatherData.value = null;
  }
};
</script>

<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Type city's name here"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
    </div>

    <div v-if="weatherData" class="bg-weather-secondary p-6 rounded-lg shadow-lg mt-4 text-center">
      <h2 class="text-3xl font-bold mb-2">{{ weatherData.location.name }}, {{ weatherData.location.country }}</h2>
      <img :src="weatherData.current.condition.icon" alt="Weather Icon" class="mx-auto mb-2" />
      <p class="text-xl capitalize">{{ weatherData.current.condition.text }}</p>
      <p class="text-4xl font-bold my-2">{{ weatherData.current.temp_c }}°C</p>
      
      <div class="flex gap-4 justify-center mt-4">
        <div>
          <p>💧 Humidity: {{ weatherData.current.humidity }}%</p>
          <p>🌬️ Wind Speed: {{ weatherData.current.wind_kph }} kph</p>
        </div>
        <div>
          <p>🌅 Feels like: {{ weatherData.current.feelslike_c }}°C</p>
        </div>
      </div>
    </div>

    <p v-else class="text-center text-gray-400">Search for a city ! :)</p>
  </main>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: auto;
}
</style>
