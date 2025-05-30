<template>
  <section id="event-location" class="event-location-section">
    <h2 class="section-title">Detalles del gran Día</h2>
    <div class="event-info">
      <div class="event-card col-6" v-for="place in places" :key="place.name">
        <h3 class="place-name">{{ place.name }}</h3>
        <p class="place-date">{{ place.date }}</p>
        <p class="place-address">{{ place.address }}</p>
        <iframe
          class="map-frame"
          :src="place.mapUrl"
          width="100%"
          height="250"
          style="border:0"
          allowfullscreen=""
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
        ></iframe>
      </div>
    </div>
    <p class="note">
      Lo más importante es compartir este momento contigo.<br />
      <strong>Sugerimos vestimenta formal</strong>
    </p>
    <a class="rsvp-button" href="#/rsvp" >Confirmar asistencia</a>

  </section>
</template>

<script setup>
const places = [
  {
    name: 'Ceremonia',
    date: '2 de Agosto, 2025 - 5:30 PM',
    address: 'Iglesia Santa Teresita, Av. 6 de Diciembre y Tarqui, Quito, Ecuador',
    mapUrl:
      'https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3979.9596929314725!2d-78.4985258!3d-0.2046501!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x91d59a15bfbb8737%3A0xe74682555dac7b53!2sIglesia%20Cat%C3%B3lica%20Santa%20Teresita!5e0!3m2!1ses!2sec!4v1700000000000!5m2!1ses!2sec'
  },
  {
    name: 'Recepción',
    date: 'JW Marriott Hotel Salon Amazonas',
    address: 'Av. Orellana y Amazonas, Quito, Ecuador',
    mapUrl:
      'https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3979.952105104173!2d-78.48894558524104!3d-0.19705589990047016!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x91d59a6c52cb1a2d%3A0x4ad8e2b07be0c865!2sJW%20Marriott%20Hotel%20Quito!5e0!3m2!1ses!2sec!4v1700000000000!5m2!1ses!2sec'
  }
];

function goToReservation() {
  window.location.href = '/rsvp';
}

function scrollToNext() {
  const next = document.getElementById('timeline-section');
  if (next) next.scrollIntoView({ behavior: 'smooth' });
}
</script>

<style scoped>
.event-location-section {
    background-image: url('/images/hero-bg.jpg');
        background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  padding: 2rem 1rem;
}

.section-title {
  font-size: 2.5rem;
  color: #4f3d1a;
  font-family: 'Alex Brush', cursive;
  margin-bottom: 2rem;
  animation: slideFadeIn 1s ease-out;
}

.event-info {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  justify-content: center;
  width: 100%;
}

.event-card {
  flex: 1 1 calc(50% - 2rem);
  max-width: 500px;
  background-color: #fffaf0;
  border: 1px solid #e0cda9;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.place-name {
  font-size: 1.8rem;
  color: #b68d21;
  margin-bottom: 0.5rem;
}

.place-date {
  font-weight: 600;
  color: #4f3d1a;
  margin-bottom: 0.5rem;
}

.place-address {
  font-size: 1rem;
  color: #6b4f1d;
  margin-bottom: 1rem;
}

.map-frame {
  border-radius: 0.5rem;
}

.note {
  font-size: 1.2rem;
  color: #4f3d1a;
  text-align: center;
  max-width: 600px;
  margin-top: 1rem;
  padding: 0 1rem;
  font-style: italic;
}

.note strong {
  font-weight: 600;
  color: #6b4f1d;
}

.rsvp-button {
  margin-top: 1rem;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  background-color: #b68d21;
  color: white;
  border: none;
  border-radius: 2rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.rsvp-button:hover {
  background-color: #a77b19;
}

.scroll-arrow {
  margin-top: 2rem;
  cursor: pointer;
  animation: bounce 2s infinite;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #4f3d1a;
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
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(10px); }
}

@keyframes slideFadeIn {
  0% {
    opacity: 0;
    transform: translateY(-30px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .event-info {
    flex-direction: column;
    align-items: center;
  }

  .event-card {
    flex: 1 1 100%;
    max-width: 100%;
  }

  .section-title {
    font-size: 2rem;
  }
}
</style>
