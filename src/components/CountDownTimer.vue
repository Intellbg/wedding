<template>
  <div class="countdown-wrapper">
    <div class="countdown-text">
      Faltan {{ days }} días, {{ hours }}h {{ minutes }}m {{ seconds }}s
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const targetDate = new Date('2025-08-02T05:30:00');
const days = ref(0);
const hours = ref(0);
const minutes = ref(0);
const seconds = ref(0);

let interval = null;

const updateCountdown = () => {
  const now = new Date();
  const distance = targetDate - now;

  if (distance < 0) {
    clearInterval(interval);
    days.value = hours.value = minutes.value = seconds.value = 0;
    return;
  }

  days.value = Math.floor(distance / (1000 * 60 * 60 * 24));
  hours.value = Math.floor((distance / (1000 * 60 * 60)) % 24);
  minutes.value = Math.floor((distance / 1000 / 60) % 60);
  seconds.value = Math.floor((distance / 1000) % 60);
};

onMounted(() => {
  updateCountdown();
  interval = setInterval(updateCountdown, 1000);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<style scoped>
.countdown-wrapper {
  padding: 1rem;
  border-radius: 1rem;
  font-size: 1.5rem;
  font-weight: bold;
  color: #4f3d1a;
  text-align: center;
  max-width: 90%;
  margin: 0 auto;
}
</style>
