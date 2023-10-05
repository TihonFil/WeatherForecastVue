<script setup>
import { capitalizeFirstLetter } from '../utils/';
import { ref, computed } from 'vue';

const props = defineProps({
  weatherInfo: {
    type: [Object, null],
    required: true,
  }
});

const formattedDate = computed(() => {
  const timestamp = props.weatherInfo?.dt;
  const timeZone = props.weatherInfo?.timezone;
  const date = new Date((timestamp + timeZone) * 1000);
  const optionsDate = { weekday: 'short', year: 'numeric', month: 'long', day: 'numeric' };
  return date.toLocaleDateString('ru-RU', optionsDate);
});

</script>

<template>
  <div class="summary">
    <div class="pic-main"><img
        class="pic-main__img"
        :src="`https://openweathermap.org/img/wn/${weatherInfo?.weather[0].icon}@2x.png`"
        alt="pop"
      ></div>
    <div class="weather">
      <div class="temp">
        {{ Math.round(weatherInfo?.main.temp) }} °C
      </div>
      <div class="weather-desc text-block">
        {{ capitalizeFirstLetter(weatherInfo?.weather[0].description) }}
      </div>
    </div>
    <div class="city text-block">
      {{ weatherInfo?.name }},
      {{ weatherInfo?.sys?.country }}
    </div>
    <div class="date text-block">
      {{ formattedDate }}
    </div>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.pic-main
  width: 100%
  height: 100%

.pic-main__img
  max-width: 100px
  max-height: 100px

.city
  font-size: 24px

.weather
  margin: 0 0 20px
  padding: 20px 0
  border-bottom: 1px solid rgba(255, 255, 255, 0.4)

.temp
  padding-bottom: 8px
  font-size: 32px

.text-block
  position: relative
  padding-left: 24px
  padding-bottom: 8px
  font-size: 14px

  &::before
    content: ''
    position: absolute
    top: 0
    left: 0
    width: 20px
    height: 20px
    margin-right: 6px
    background-repeat: no-repeat
    background-position: 50% 50%
    background-size: contain

.weather-desc

  &::before
    background-image: url('/src/assets/img/weather.svg')

.city

  &::before
    background-image: url('/src/assets/img/location.svg')

.date

  &::before
    left: 2px
    width: 15px
    height: 15px
    background-image: url('/src/assets/img/calendar.svg')
</style>