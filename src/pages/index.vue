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
  <NavButton/>
</template>

<script setup>
// No logic needed here
</script>

<style>
/* Floating navigation button */
.nav-floating {
  position: fixed;
  bottom: 20px;
  left: 20px;
  z-index: 999;
  font-family: 'Playfair Display', serif;
}

/* Circular gold button with subtle glassy effect */
.nav-btn {
  background: linear-gradient(145deg, #d4af37, #b68d21);
  color: white;
  border: none;
  border-radius: 50%;
  width: 52px;
  height: 52px;
  font-size: 1.4rem;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.15);
  transition: background 0.3s ease, transform 0.2s ease;
  backdrop-filter: blur(6px);
}

.nav-btn:hover {
  background: linear-gradient(145deg, #caa331, #9e7f1b);
  transform: scale(1.05);
}

/* Elegant popup menu */
.nav-popup {
  position: absolute;
  bottom: 70px;
  left: 0;
  background: rgba(255, 253, 247, 0.96);
  border: 1.5px solid #c2a74e;
  border-radius: 12px;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
  padding: 0.8rem 1.2rem;
  min-width: 180px;
  backdrop-filter: blur(8px);
  animation: slideUp 0.3s ease-out;
}

/* Menu list */
.nav-popup ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.nav-popup li {
  margin: 0.6rem 0;
}

/* Menu links */
.nav-popup a {
  color: #6b4f1d;
  text-decoration: none;
  font-weight: 600;
  font-size: 1rem;
  display: inline-block;
  position: relative;
}

.nav-popup a::after {
  content: "";
  position: absolute;
  width: 0%;
  height: 2px;
  left: 0;
  bottom: -2px;
  background-color: #b68d21;
  transition: width 0.3s ease;
}

.nav-popup a:hover::after {
  width: 100%;
}

/* Fade + Slide Up transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@keyframes slideUp {
  from {
    transform: translateY(10px);
    opacity: 0;
  }
  to {
    transform: translateY(0px);
    opacity: 1;
  }
}

/* Optional: smaller screen adjustments */
@media (max-width: 600px) {
  .nav-popup {
    bottom: 80px;
    min-width: 150px;
    padding: 0.6rem 1rem;
  }

  .nav-btn {
    width: 46px;
    height: 46px;
    font-size: 1.2rem;
  }
}

</style>
