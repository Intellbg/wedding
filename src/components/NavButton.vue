<template>
    <RouterView />
  
    <!-- Floating Navigation Button -->
    <div class="nav-floating">
      <button class="nav-btn" @click="isNavOpen = !isNavOpen" aria-label="Menú de navegación">
        ☰
      </button>
      <transition name="fade">
        <div v-if="isNavOpen" class="nav-popup">
          <ul>
            <li><a href="#app" @click="isNavOpen = false">Invitación</a></li>
            <li><a href="#timeline-section" @click="isNavOpen = false">Nuestra Historia</a></li>
            <li><a href="#event-location" @click="isNavOpen = false">Detalles</a></li>
            <li><a href="/rsvp" @click="isNavOpen = false">RSVP</a></li>
          </ul>
        </div>
      </transition>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  const isNavOpen = ref(false)
  </script>
  
  <style>
.nav-floating {
  position: fixed;
  bottom: 20px;
  left: 20px;
  z-index: 999;
}
.nav-btn {
  background-color: #b68d21;
  color: white;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  font-size: 1.5rem;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
.nav-popup {
  position: absolute;
  bottom: 60px;
  left: 0;
  background: white;
  border: 2px solid #b68d21;
  border-radius: 10px;
  box-shadow: 0 6px 12px rgba(0,0,0,0.15);
  padding: 0.5rem 1rem;
}
.nav-popup ul {
  list-style: none;
  padding: 0;
}
.nav-popup li {
  margin: 0.5rem 0;
}
.nav-popup a {
  color: #4f3d1a;
  text-decoration: none;
  font-weight: 500;
}
.nav-popup a:hover {
  text-decoration: underline;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>