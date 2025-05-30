<template>
  <div class="snap-container">
    <section class="snap-section">
      <Header />
    </section>
    <section class="snap-section">
      <LocationSection />
    </section>
    <section class="snap-section">
      <Timeline />
    </section>
  </div>
</template>


<script setup>
</script>

<style>
.snap-container {
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
}

.snap-section {
  scroll-snap-align: start;
  height: 100vh;
  width: 100%;
}

</style>