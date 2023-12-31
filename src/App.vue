<script setup>
import WeatherSummary from './components/WeatherSummary.vue';
import Highlights from './components/Highlights.vue';
import Coords from './components/Coords.vue';
import Humidity from './components/Humidity.vue';
import Time from './components/Time.vue';

import { capitalizeFirstLetter } from './utils'
import { API_KEY, BASE_URL, FORECAST_BASE_URL } from './constants';

import { ref, onMounted, computed } from 'vue';

const isError = computed(() => weatherInfo.value?.cod !== 200)
const sectionClass = computed(() => {
  if (isError.value) {
    return 'section section-error';
  } else {
    return 'section section-left';
  }
});

const city = ref('Нови-Сад'); // самоа Никарагуа
const weatherInfo = ref(null);
const forecastInfo = ref(null);

function getWeather() {
  fetch(`${BASE_URL}?q=${city.value}&lang=ru&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => weatherInfo.value = data)
}

onMounted(getWeather);

</script>

<template>
  <div class="page">
    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="sectionClass">
              <div class="info">
                <div class="city-inner">
                  <input
                    type="text"
                    class="search"
                    v-model="city"
                    @keyup.enter="getWeather"
                  >
                  <span
                    class="input-search"
                    @click="getWeather"
                  ></span>
                </div>
                <WeatherSummary
                  v-if="!isError"
                  :weatherInfo="weatherInfo"
                />
                <div
                  v-else
                  class="error"
                >
                  <div class="error-title">
                    Ооооопс! Что-то пошло не так!
                  </div>
                  <div
                    v-if="weatherInfo?.message"
                    class="error-message"
                  >
                    {{ capitalizeFirstLetter(weatherInfo?.message) }}
                  </div>
                </div>
                <div
                  v-if="!isError"
                  class="time-wrapper"
                >
                  <Time :weatherInfo="weatherInfo" />
                </div>
              </div>
            </section>
            <section
              v-if="!isError"
              class="section section-right"
            >
              <Highlights :weatherInfo="weatherInfo" />
            </section>
          </div>
          <div
            v-if="!isError"
            class="sections"
          >
            <Coords :coord="weatherInfo.coord" />
            <Humidity :humidity="weatherInfo.main" />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="sass" scoped>
@import './assets/styles/main'
.page
  position: relative
  display: flex
  justify-content: center
  align-items: center
  min-height: 100vh
  padding: 20px 0
  background-color: #ECEEF2

  @media (max-width: 575px)
    padding: 0

.laptop
  width: 100%
  padding: 20px
  background-color: #b9b9c9
  border-radius: 25px

  @media (max-width: 575px)
    border-radius: 0

.sections
  display: flex
  justify-content: space-between
  width: 100%

  @media (max-width: 767px)
    flex-direction: column

.section-left
  width: 30%
  padding-right: 10px

  @media (max-width: 767px)
    width: 100%
    padding-right: 0

  &.section-error
    width: auto
    min-width: 235px
    padding-left: 0

.section-right
  width: 70%
  padding-left: 10px

  @media (max-width: 767px)
    width: 100%
    margin-top: 16px
    padding-left: 0

.city-inner
  position: relative
  display: inline-block
  width: 100%

.input-search
  content: ''
  position: absolute
  top: 0
  right: 10px
  width: 25px
  height: 25px
  background: url('./assets/img/search.svg') no-repeat 50% 50%
  background-size: contain
  transform: translateY(50%)
  cursor: pointer

.info
  height: 100%
  padding: 16px
  background-color: #9494ac
  border-radius: 25px

.search
  width: 100%
  padding: 16px
  font-family: 'Inter', Arial, sans-serif
  color: $white
  background-color: rgb(101 85 140 / 75%)
  border-radius: 16px
  border: none
  outline: none

.section-bottom
  width: 49%
  margin-top: 16px

  @media (max-width: 767px)
    width: 100%

.error
  &-title
    font-size: 18px
    font-weight: 700
    margin: 20px 0

  &-message
    font-size: 16px
</style>
