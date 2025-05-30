<template>
    <v-container fluid class="pa-0 hero-container">
        <v-sheet class="hero-sheet d-flex flex-column justify-center align-center text-center">
            <!-- Hero Content -->
            <div class="hero-content">
                <div>
                    <h1 class="title font-weight-bold mb-4 fade-in delay-1 gold-text">
                        ¡Estás Invitado!
                    </h1>
                     
                    <h2 class="subtitle font-italic-bold mb-2 fade-in delay-2 brown-text">
                        A Nuestro Gran Día
                    </h2>
                </div>

                <!-- Image Overlay Section -->
                <div class="image-overlay-container fade-in delay-3">
                    <img src="/images/world-map.png" alt="World Map" class="map-img" />
                    <img src="/images/compass.png" alt="Compass" class="compass-img centered-compass" />
                </div>
                <CountDownTimer class='fade-in delay-3'></CountDownTimer>

                <h2 class="cursive names mb-2 delay-2 brown-text typing-text">
                </h2>
            </div>


            <!-- Scroll Down Arrow -->
            <div class="scroll-arrow brown-text" @click="scrollToNext" aria-label="Scroll to next section">
                <svg width="32" height="32" viewBox="0 0 24 24" class="arrow-icon">
                    <path d="M12 5v14M12 19l-7-7M12 19l7-7" stroke="currentColor" stroke-width="2"
                        stroke-linecap="round" stroke-linejoin="round" fill="none" />
                </svg>
            </div>
        </v-sheet>
    </v-container>
</template>

<script setup>
function scrollToNext() {
    const section = document.getElementById('timeline-section');
    if (section) {
        section.scrollIntoView({ behavior: 'smooth' });
    }
}
</script>

<style scoped>
/* Utility Colors */
.brown-text {
    color: #4f3d1a;
}

.gold-text {
    color: #b68d21;
}

/* Layout */
.hero-container,
.hero-sheet {
    background-image: url('/images/hero-bg.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    position: relative;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100vh;
    padding: 5vh 0;
}



.hero-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
    height: 100%;
    width: 100%;
}


/* Fade-in and slide-up animation */
.fade-in {
    opacity: 0;
    transform: translateY(40px);
    animation: fadeUp 0.8s ease-out forwards;
}

.delay-1 {
    animation-delay: 0.4s;
}

.delay-2 {
    animation-delay: 0.8s;
}

.delay-3 {
    animation-delay: 1.2s;
}

.delay-4 {
    animation-delay: 1.6s;
}

@keyframes fadeUp {
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Typography */
.title {
    font-size: clamp(2.5rem, 6vw, 5rem);
}

.subtitle {
    font-size: clamp(1.8rem, 4.5vw, 3.5rem);
}

@keyframes typewriter {
  0% { content: ""; }
  5% { content: "A"; }
  10% { content: "Al"; }
  15% { content: "Ale"; }
  20% { content: "Alej"; }
  25% { content: "Aleja"; }
  30% { content: "Alejan"; }
  35% { content: "Alejand"; }
  40% { content: "Alejandr"; }
  45% { content: "Alejandra"; }
  50% { content: "Alejandra "; }
  55% { content: "Alejandra Y"; }
  60% { content: "Alejandra Y "; }
  65% { content: "Alejandra Y N"; }
  70% { content: "Alejandra Y Ní"; }
  75% { content: "Alejandra Y Níc"; }
  80% { content: "Alejandra Y Níco"; }
  85% { content: "Alejandra Y Nícól"; }
  90% { content: "Alejandra Y Nícul"; }
  95% { content: "Alejandra Y Nicolás"; }
  100% { content: "Alejandra Y Nicolás"; }
}

.typing-text::after {
  content: "";
  animation: typewriter 2s steps(1, end) forwards;
  white-space: nowrap;
  font-family: 'Alex Brush', cursive;
  font-size: clamp(2.5rem, 6vw, 5rem);
  color: #4f3d1a;
}

.names {
    font-family: "Alex Brush", cursive;
    font-style: italic;
    font-size: clamp(2.5rem, 6vw, 5rem);
}

/* Image overlay */
.image-overlay-container {
    position: relative;
    width: 95vw;
    max-width: 700px;
    margin: 1rem auto 0.5rem;
}

.map-img {
    width: 100%;
    height: auto;
    display: block;
}

.compass-img.centered-compass {
    position: absolute;
    width: 40%;
    height: auto;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation: compassWobble 6s ease-in-out infinite;
}

@keyframes compassWobble {
  0% {
    transform: translate(-50%, -50%) rotate(0deg);
  }
  10% {
    transform: translate(-50%, -50%) rotate(20);
  }
  20% {
    transform: translate(-50%, -50%) rotate(-15deg);
  }
  30% {
    transform: translate(-50%, -50%) rotate(20deg);
  }
  40% {
    transform: translate(-50%, -50%) rotate(-1deg);
  }
  50% {
    transform: translate(-50%, -50%) rotate(0.5deg);
  }
  60% {
    transform: translate(-50%, -50%) rotate(-0.3deg);
  }
  70% {
    transform: translate(-50%, -50%) rotate(0.2deg);
  }
  80% {
    transform: translate(-50%, -50%) rotate(-0.1deg);
  }
  90% {
    transform: translate(-50%, -50%) rotate(0.05deg);
  }
  100% {
    transform: translate(-50%, -50%) rotate(0deg);
  }
}

to {
    transform: translate(-50%, -50%) rotate(360deg);
}

/* Scroll arrow */
.scroll-arrow {
    position: absolute;
    bottom: 10px;
    cursor: pointer;
    animation: bounce 2s infinite;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
}

.arrow-icon {
    width: 2rem;
    height: 2rem;
    transition: transform 0.3s ease;
}

.scroll-arrow:hover .arrow-icon {
    transform: translateY(5px);
}

@keyframes bounce {

    0%,
    100% {
        transform: translateY(0);
    }

    50% {
        transform: translateY(10px);
    }
}
</style>
