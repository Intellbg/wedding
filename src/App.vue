<template>
  <v-app>
    <v-main>
      <router-view />
      <audio ref="introAudio" src="/audio/intro.mp3" preload="auto" />
    </v-main>
  </v-app>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const introAudio = ref(null);

onMounted(() => {
  const audio = introAudio.value;
  if (audio) {
    // Try to autoplay; browsers might block this without user interaction
    audio.play().catch(() => {
      console.warn('Autoplay blocked. Will play on user interaction.');
      // Optional: Play on user gesture
      const resume = () => {
        audio.play();
        window.removeEventListener('click', resume);
      };
      window.addEventListener('click', resume);
    });
  }
});

function scrollToNext() {
  const section = document.getElementById('next-section');
  if (section) {
    section.scrollIntoView({ behavior: 'smooth' });
  }
}
</script>
