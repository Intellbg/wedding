<template>
  <section id="timeline-section" class="timeline-section">
    <div class="timeline-header">
      <h2 class="section-title">Nuestro viaje hasta el momento</h2>
    </div>
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
      <div
        class="timeline-content-box"
        @touchstart="startTouch"
        @touchend="endTouch"
      >
        <transition name="fade-slide" mode="out-in">
          <div v-if="selectedEvent" :key="selectedEvent.year">
            <p class="description">{{ selectedEvent.description }}</p>
            <div class="gallery">
              <div class="photo" v-for="(photo, i) in selectedEvent.photos" :key="i">
                <img :src="photo" :alt="'Foto de ' + selectedEvent.year" loading="lazy" />
              </div>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </section>

  <!-- Dummy next section -->
  <section id="next-section" style="height: 100vh; background: #f0f0f0;">
    <h2 style="padding-top: 2rem; text-align: center;">¡Bienvenidos a la siguiente sección 🎉!</h2>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

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
  { year: '2025', description: '¡Planeando nuestra boda!', photos: ['/images/timeline/2024.jpg'] },
]

const selectedIndex = ref(0)
const selectedEvent = computed(() => events[selectedIndex.value])

function selectYear(index) {
  selectedIndex.value = index
}

// Swipe logic
const touchStartX = ref(0)
const touchEndX = ref(0)

function startTouch(e) {
  touchStartX.value = e.changedTouches[0].screenX
}

function endTouch(e) {
  touchEndX.value = e.changedTouches[0].screenX
  handleSwipe()
}

function handleSwipe() {
  const distance = touchEndX.value - touchStartX.value
  const threshold = 50

  if (Math.abs(distance) > threshold) {
    if (distance < 0) {
      // Swipe left → next
      if (selectedIndex.value < events.length - 1) {
        selectedIndex.value++
      } else {
        scrollToNextSection()
      }
    } else {
      // Swipe right ← previous
      if (selectedIndex.value > 0) {
        selectedIndex.value--
      }
    }
  }
}

function scrollToNextSection() {
  const nextSection = document.querySelector('#next-section')
  if (nextSection) {
    nextSection.scrollIntoView({ behavior: 'smooth' })
  }
}
</script>

<style scoped>
.timeline-section {
  display: flex;
  flex-direction: column;
  height: 100vh;
  padding: 2rem;
  background: #fffdf7;
  justify-content: space-between;
  text-align: center;
  overflow: hidden;
  flex-wrap: nowrap;
}

.section-title {
  margin-bottom: 1rem;
  font-size: 2.5rem;
  color: #4f3d1a;
  font-family: 'Alex Brush', cursive;
  text-align: center;
  width: 100%;
  animation: slideFadeIn 1s ease-out;
}

.timeline-row {
  display: flex;
  flex-direction: row;
  gap: 2rem;
  flex: 1;
  width: 100%;
  justify-content: center;
  align-items: flex-start;
}

.timeline-vertical {
  margin-top: 1rem;
  align-self: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.5rem;
  overflow-y: auto;
  max-height: 80vh;
  padding: 1rem;
  border-right: 2px solid #b68d21;
  scrollbar-width: thin;
  flex: 0 0 80px;
}

.timeline-node {
  padding: 0.3rem;
  background: #fffaf0;
  border: 2px solid #b68d21;
  border-radius: 50px;
  cursor: pointer;
  transition: transform 0.3s ease, background-color 0.3s ease;
  outline: none;
  font-weight: bold;
  color: #6b4f1d;
  font-size: 0.95rem;
  text-align: center;
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
  margin-top: 1rem;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  max-height: 80vh;
  overflow-y: auto;
  padding: 2rem;
  animation: fadeIn 0.4s ease;
  flex: 2;
  min-width: 300px;
}

.description {
  font-size: 1.3rem;
  color: #4f3d1a;
  max-width: 600px;
  margin: 1rem auto;
  font-weight: 500;
  line-height: 1.6;
}

.gallery {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
}

.photo img {
  width: 100%;
  max-width: 220px;
  height: auto;
  object-fit: cover;
  border-radius: 0.75rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  transition: transform 0.3s ease;
}

.photo img:hover {
  transform: scale(1.05);
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
  .timeline-section {
    flex-direction: column;
    min-height: 100vh;
    align-items: center;
    height: auto;
    padding: 1rem;
  }

  .section-title {
    font-size: 2rem;
    margin-bottom: 1.5rem;
  }

  .gallery {
    gap: 0.5rem;
  }

  .timeline-vertical {
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    border-right: none;
    border-bottom: 2px solid #b68d21;
    max-height: none;
    flex: none;
  }

  .timeline-node {
    min-width: 50px;
    padding: 0.3rem 0.5rem;
    font-size: 0.85rem;
  }

  .timeline-content-box {
    padding: 1rem;
  }

  .photo img {
    max-width: 100%;
    height: auto;
  }

  .timeline-row {
    flex-direction: column;
    align-items: center;
  }
}
</style>
