<script setup>
  import { ref, onMounted, computed } from 'vue';
  import { API_KEY, BASE_URL } from './constants';

  import WeatherSummary from './components/WeatherSummary.vue';
  import Highlights from './components/Highlights.vue';
  import Coords from './components/Coords.vue';
  import Humidity from './components/Humidity.vue';

  const city = ref('');
  const weatherInfo = ref(null);
  const isError = computed(() => weatherInfo.value?.cod !== 200);


  const saveLastCityToLocalStorage = (city) => {
    localStorage.setItem('lastCity', city);
  };

  const getWeather = async () => {
    await fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
      .then((response) => response.json())
      .then((data) => {
        weatherInfo.value = data;
        if (!isError.value) {
          saveLastCityToLocalStorage(city.value);
        }
      });
  };

  onMounted(() => {
    // При загрузке страницы попробуем получить последний сохраненный город и установить его в поле city.
    const lastCity = localStorage.getItem('lastCity');
    if (lastCity) {
      city.value = lastCity;
    }

    getWeather();
  });
</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="['section', 'section-left', {'section-error': isError}]">
              <div class="info">
                <div class="city-inner">
                  <input v-model="city" @keyup.enter="getWeather" type="text" class="search">
                </div>
                <WeatherSummary v-if="!isError" :weatherInfo="weatherInfo" />
                <div v-else class="error">
                  <div class="error-title">
                    Ooooooops! <br>
                    Something went wrong
                  </div>
                  <div v-if="weatherInfo?.message" class="error-message">
                    {{ weatherInfo?.message }}
                  </div>
                </div>
              </div>
            </section>
            <section v-if="!isError" class="section section-right">
              <Highlights :weatherInfo="weatherInfo" />
            </section>
          </div>
          <div v-if="!isError" class="sections">
            <Coords :coord="weatherInfo.coord" />
            <Humidity :humidity="weatherInfo.main.humidity"  />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped></style>
