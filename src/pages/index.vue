<template>
  <main class="snap-container">
    <!-- Sections that snap -->
    <section class="snap-section-mobile">
      <Header />
    </section>
    <section class="snap-section-mobile">
      <Timeline />
    </section>
    <section class="snap-section">
      <LocationSection />
    </section>
  </main>
</template>

<script setup>
// No logic needed here
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
}

.snap-container {
  height: 100vh;
  overflow-y: auto;
  scroll-behavior: smooth;
  -webkit-overflow-scrolling: touch;
}

.snap-section-mobile {
  height: 100vh;
  width: 100%;
}

.snap-section {
  height: 100vh;
  width: 100%;
  scroll-snap-align: start;
}

@media (min-width: 768px) {
  .snap-container {
    scroll-snap-type: y mandatory;
  }

  .snap-section-mobile {
    scroll-snap-align: start;
  }
}
</style>
