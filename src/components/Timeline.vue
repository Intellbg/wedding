<template>
  <section id="timeline-section" class="timeline-section">
    <h2 class="section-title">Nuestro viaje hasta el momento</h2>
    <div class="timeline-row">
      <div class="timeline-vertical">
        <div
          v-for="(event, index) in events"
          :key="index"
          class="timeline-node"
          :class="{ active: selectedIndex === index }"
          @click="selectYear(index)"
          tabindex="0"
          @keydown.enter="selectYear(index)"
        >
          <span class="year">{{ event.year }}</span>
        </div>
      </div>
      <div class="timeline-content-box">
        <transition name="fade-slide" mode="out-in">
          <div v-if="selectedEvent" :key="selectedEvent.year">
            <p class="description">{{ selectedEvent.description }}</p>
            <div class="photo">
              <img
                :src="selectedEvent.photos[photoIndex]"
                :alt="'Foto de ' + selectedEvent.year"
                loading="lazy"
              />
            </div>
          </div>
        </transition>
      </div>
    </div>
    <div class="scroll-arrow brown-text" @click="scrollToNext" aria-label="Scroll to next section">
      <svg width="32" height="32" viewBox="0 0 24 24" class="arrow-icon">
        <path
          d="M12 5v14M12 19l-7-7M12 19l7-7"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          fill="none"
        />
      </svg>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const events = [
  { year: '2015', description: 'Y así todo empezó', photos: ['/images/timeline/2015.jpg'] },
  { year: '2016', description: 'En nuestros primeros logros', photos: ['/images/timeline/2016.jpg'] },
  { year: '2017', description: 'La distancia no fue impedimento', photos: ['/images/timeline/2017.jpg'] },
  { year: '2018', description: 'Una nueva cotidianidad', photos: ['/images/timeline/2018.jpg'] },
  { year: '2019', description: 'Las tormentas pasan', photos: ['/images/timeline/2019.jpg'] },
  { year: '2020', description: 'Se acabó el encierro', photos: ['/images/timeline/2020.jpg'] },
  { year: '2021', description: 'Just chilling', photos: ['/images/timeline/2021.jpg'] },
  { year: '2022', description: 'Aventuras', photos: ['/images/timeline/2022.jpg'] },
  { year: '2023', description: 'Más Aventuras', photos: ['/images/timeline/2023.jpg'] },
  { year: '2024', description: 'Dijo que sí', photos: ['/images/timeline/2024.jpg'] },
  { year: '2025', description: '¡Planeando nuestra boda!', photos: ['/images/timeline/2025.jpg'] },
]

const selectedIndex = ref(0)
const selectedEvent = computed(() => events[selectedIndex.value])
const photoIndex = ref(0)

let autoAdvanceInterval = null

function selectYear(index) {
  selectedIndex.value = index
  photoIndex.value = 0
  resetAutoAdvance()
}

function scrollToNext() {
  const section = document.getElementById('event-location')
  if (section) {
    section.scrollIntoView({ behavior: 'smooth' })
  }
}

function setupAutoAdvance() {
  autoAdvanceInterval = setInterval(() => {
    selectedIndex.value = (selectedIndex.value + 1) % events.length
    photoIndex.value = 0
  }, 5000)
}

function resetAutoAdvance() {
  if (autoAdvanceInterval) clearInterval(autoAdvanceInterval)
  setupAutoAdvance()
}

onMounted(() => {
  setupAutoAdvance()
})

onBeforeUnmount(() => {
  if (autoAdvanceInterval) clearInterval(autoAdvanceInterval)
})
</script>

<style scoped>
.timeline-section {
  display: flex;
  flex-direction: column;
  height: 100vh;
  background: #fffdf7;
  text-align: center;
  overflow: hidden;
  scroll-snap-align: start;
  padding: 0;
}

.section-title {
  font-size: 2.5rem;
  color: #4f3d1a;
  font-family: 'Alex Brush', cursive;
  margin: 1rem auto 0.5rem;
  animation: slideFadeIn 1s ease-out;
}

.timeline-row {
  display: flex;
  flex: 1;
  width: 100%;
  overflow: hidden;
  flex-direction: row;
  flex-wrap: wrap;
}

.timeline-vertical {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  gap: 0.5rem;
  padding: 1rem;
  border-right: 2px solid #b68d21;
  flex: 0 0 80px;
  background-color: #fffdf7;
  height: 100%;
}

.timeline-node {
  padding: 0.3rem;
  background: #fffaf0;
  border: 2px solid #b68d21;
  border-radius: 50px;
  cursor: pointer;
  font-weight: bold;
  font-size: 0.95rem;
  color: #6b4f1d;
  text-align: center;
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.timeline-node:hover,
.timeline-node:focus {
  background-color: #f5e6b8;
  color: #000;
}

.timeline-node.active {
  background-color: #b68d21;
  color: white;
  transform: scale(1.1);
}

.timeline-content-box {
  flex: 1;
  padding: 1rem 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow-y: auto;
  width: 100%;
  box-sizing: border-box;
}

.description {
  font-size: 1.3rem;
  color: #4f3d1a;
  max-width: 600px;
  font-weight: 500;
  line-height: 1.6;
  margin: 1rem auto;
}

.photo {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: auto;
  padding: 1rem 0;
}

.photo img {
  max-height: 60vh;
  max-width: 100%;
  height: auto;
  object-fit: contain;
  border-radius: 0.75rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  transition: transform 0.3s ease;
}

.photo img:hover {
  transform: scale(1.05);
}

.scroll-arrow {
  padding: 1rem;
  cursor: pointer;
  color: #b68d21;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(6px);
  }
}

.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.5s ease;
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

@media (max-width: 768px) {
  .timeline-row {
    flex-direction: column;
    align-items: center;
  }

  .timeline-vertical {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    border-right: none;
    border-bottom: 2px solid #b68d21;
    width: 100%;
    padding: 0.5rem;
    height: auto;
  }

  .timeline-content-box {
    padding: 1rem;
    z-index: 0;
  }

  .photo img {
    max-width: 100%;
    max-height: 50vh;
  }
}
</style>
