<script setup>
import { ref, onMounted, computed } from 'vue';

const props = defineProps({
  weatherInfo: {
    type: [Object, null],
    required: true,
  }
});

const currentTime = ref('');
const timeZoneOffset = computed(() => props.weatherInfo?.timezone || 0);

function getCurrentTime() {
  const now = new Date(Date.now() + timeZoneOffset.value * 1000); // Преобразование в миллисекунды
  const hours = String(now.getUTCHours()).padStart(2, '0');
  const minutes = String(now.getUTCMinutes()).padStart(2, '0');
  const seconds = String(now.getUTCSeconds()).padStart(2, '0');
  currentTime.value = `${hours}:${minutes}:${seconds}`;
}

onMounted(() => {
  getCurrentTime();
  setInterval(getCurrentTime, 1000);
});
</script>

<template>
  <div class="time">
    <span>Местное время - </span>
    {{ currentTime }}
  </div>
</template>

<style lang="sass" scoped>
.time
  display: block
  font-size: 18px
  font-weight: 700
  & span
    font-size: 14px
    font-weight: 400

</style>
