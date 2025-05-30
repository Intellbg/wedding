<template>
  <section id="rsvp" class="rsvp-section">
    <h2 class="section-title">Confirme su Asistencia</h2>
    <div class="rsvp-card">
      <form @submit.prevent="submitRSVP" class="rsvp-form">
        <label>
          Nombre Completo
          <div class="input-wrapper">
            <input type="text" v-model="form.name" required />
            <span class="info-icon" @mouseenter="showInfo = true" @mouseleave="showInfo = false">&#9432;</span>
            <transition name="fade">
              <small class="input-helper" v-if="showInfo">
                Ingrese el primer nombre y apellido tal como aparece en la invitación, sin abreviaciones.
              </small>
            </transition>
          </div>
        </label>

        <label>
          ¿Asistirás?
          <select v-model="form.attendance" required>
            <option value="Sí">Sí</option>
            <option value="No">No</option>
          </select>
        </label>

        <label v-if="form.attendance === 'Sí'">
          Número de personas
          <input type="number" v-model="form.peopleCount" min="1" required />
        </label>

        <button type="submit" class="submit-btn">Enviar</button>
      </form>

      <p class="rsvp-note">
        Si tiene algún inconveniente al confirmar, por favor
        <a href="https://wa.me/593991391910" target="_blank" class="whatsapp-link">
          contáctanos por WhatsApp
        </a>
      </p>
      <a href="/" class="rsvp-note">Regresar</a>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref } from 'vue'

const form = reactive({
  name: '',
  attendance: '',
  peopleCount: 1
})

const showInfo = ref(false)

async function submitRSVP() {
  try {
    const response = await fetch('https://api_wedding.usagicli.com/api/rsvp', {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        name: form.name,
        attendance: form.attendance,
        peopleCount: form.attendance === 'Sí' ? form.peopleCount : 0
      })
    })

    const data = await response.json()

    if (!response.ok) {
      throw new Error(data.message || 'Ocurrió un error al confirmar.')
    }

    alert(data.message || '¡Gracias por confirmar tu asistencia!')
    form.name = ''
    form.attendance = ''
    form.peopleCount = 1
  } catch (error) {
    alert(error.message || 'No se pudo completar tu confirmación. Intenta más tarde.')
    console.error('Error en RSVP:', error)
  }
}
</script>

<style scoped>
.rsvp-section {
  min-height: 100vh;
  padding: 5rem 1.5rem 3rem;
  background-color: #fffdf7;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.section-title {
  font-size: 2.5rem;
  font-family: 'Alex Brush', cursive;
  color: #4f3d1a;
  margin-bottom: 2rem;
}

.rsvp-card {
  background: white;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  max-width: 520px;
  width: 100%;
}

.rsvp-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.rsvp-form label {
  display: flex;
  flex-direction: column;
  font-weight: bold;
  color: #4f3d1a;
}

.rsvp-form input,
.rsvp-form select {
  padding: 0.75rem;
  border-radius: 0.5rem;
  border: 1px solid #b68d21;
  margin-top: 0.5rem;
  font-size: 1rem;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  width: 100%;
}
.input-wrapper input {
  flex-grow: 1;
}
.info-icon {
  margin-left: 0.5rem;
  cursor: pointer;
  font-size: 1.2rem;
  color: #b68d21;
}

.input-helper {
  position: absolute;
  top: 100%;
  left: 0;
  background-color: #fffaf0;
  border: 1px solid #b68d21;
  border-radius: 0.5rem;
  padding: 0.5rem;
  width: 100%;
  z-index: 5;
  margin-top: 0.3rem;
}

.submit-btn {
  background-color: #b68d21;
  color: white;
  font-weight: bold;
  padding: 0.75rem;
  border: none;
  border-radius: 2rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-btn:hover {
  background-color: #a77b19;
}

.rsvp-note {
  margin-top: 2rem;
  font-size: 0.95rem;
  color: #4f3d1a;
}

.whatsapp-link {
  color: #25d366;
  font-weight: bold;
  text-decoration: none;
}

.whatsapp-link:hover {
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

@media (max-width: 768px) {
  .section-title {
    font-size: 2rem;
  }

  .rsvp-card {
    padding: 1.5rem;
  }

  .rsvp-form input,
  .rsvp-form select {
    font-size: 0.95rem;
  }
}
</style>
