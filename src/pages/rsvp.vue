<template>
  <section id="rsvp" class="rsvp-section">
    <h2 class="section-title">Confirme su Asistencia</h2>
    <div class="rsvp-card">
      <form @submit.prevent="submitRSVP" class="rsvp-form">
        <label>
          Nombre Completo
          <div class="input-wrapper autocomplete-wrapper">
            <input type="text" v-model="form.name" required class="styled-input"
              @focus="showSuggestions = suggestions.length > 0" @blur="handleBlur" />
            <span class="info-icon" @mouseenter="showInfo = true" @mouseleave="showInfo = false">
              &#9432;
            </span>
            <transition name="fade">
              <small class="input-helper" v-if="showInfo">
                Ingrese el primer nombre y apellido tal como aparece en la invitación, sin abreviaciones.
              </small>
            </transition>
            <ul v-if="showSuggestions && suggestions.length" class="suggestion-list">
              <li v-for="(s, i) in suggestions" :key="i" @click="selectSuggestion(s)">
                {{ s }}
              </li>
            </ul>
          </div>
        </label>

        <label>
          ¿Asistirá?
          <select v-model="form.attendance" required class="styled-input">
            <option value="Sí">Sí</option>
            <option value="No">No</option>
          </select>
        </label>

        <label v-if="form.attendance === 'Sí'">
          Número de personas
          <input type="number" v-model.number="form.peopleCount" min="1" required class="styled-input" />
        </label>

        <button type="submit" class="submit-btn">Enviar</button>
        <p v-if="successMessage" class="feedback success">{{ successMessage }}</p>
        <p v-if="errorMessage" class="feedback error">{{ errorMessage }}</p>

      </form>

      <p class="rsvp-note">
        Si tiene algún inconveniente al confirmar, por favor
        <a href="https://wa.me/593991391910" target="_blank" class="whatsapp-link">
          contáctenos por WhatsApp
        </a>
      </p>
      <a href="/" class="rsvp-note">Regresar</a>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref, watch } from 'vue'

const API_BASE_URL = 'http://localhost:8000/api/'

const form = reactive({
  name: '',
  attendance: '',
  peopleCount: 1
})

const showInfo = ref(false)
const suggestions = ref([])
const showSuggestions = ref(false)
const successMessage = ref('')
const errorMessage = ref('')

let nameChosen = false


function handleBlur() {
  setTimeout(() => {
    showSuggestions.value = false
  }, 200)
}

watch(() => form.name, async (newValue) => {
  if (nameChosen && newValue !== form.name) nameChosen = false;
  else if (!newValue.trim()) nameChosen = false;
  if (nameChosen) return

  if (newValue.trim().length >= 2) {
    try {
      const res = await fetch(`${API_BASE_URL}invitations/autocomplete?name=${encodeURIComponent(newValue)}`)
      const data = await res.json()
      suggestions.value = Array.isArray(data) ? data.map(entry => entry.name) : []
      showSuggestions.value = true
    } catch (err) {
      console.error('Autocomplete error:', err)
      suggestions.value = []
      showSuggestions.value = false
    }
  } else {
    suggestions.value = []
    showSuggestions.value = false
  }
})

function selectSuggestion(name) {
  form.name = name
  nameChosen = true
  suggestions.value = []
  showSuggestions.value = false
}

async function submitRSVP() {
  try {
    const payload = {
      name: form.name,
      attendance: form.attendance,
      peopleCount: form.attendance === 'Sí' ? form.peopleCount : 0
    }
    const response = await fetch(`${API_BASE_URL}rsvp`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    })

    const data = await response.json()
    if (!response.ok){
      console.log(data.non_field_errors);
      throw new Error(data.non_field_errors[0] || 'Ocurrió un error al confirmar.')
    } 
    console.log(data)
    successMessage.value = `${data.nombre}, ¡gracias por confirmar la asistencia de ${data.personas_confirmadas} persona${data.personas_confirmadas > 1 ? 's' : ''}!`
    errorMessage.value = ''
    Object.assign(form, { name: '', attendance: '', peopleCount: 1 })
  } catch (error) {
    errorMessage.value = error.message || 'No se pudo completar su confirmación. Intente más tarde.'
    successMessage.value = ''
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

.styled-input {
  padding: 0.75rem;
  border-radius: 0.5rem;
  border: 1px solid #b68d21;
  margin-top: 0.5rem;
  font-size: 1rem;
  color: #4f3d1a;
  background-color: #fffaf0;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.styled-input:focus {
  outline: none;
  border-color: #a77b19;
  box-shadow: 0 0 0 2px rgba(183, 141, 33, 0.2);
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

.autocomplete-wrapper {
  position: relative;
}

.suggestion-list {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border: 1px solid #ccc;
  border-top: none;
  max-height: 150px;
  overflow-y: auto;
  z-index: 10;
  list-style: none;
  margin: 0;
  padding: 0;
  font-size: 0.95rem;
}

.suggestion-list li {
  padding: 0.5rem;
  cursor: pointer;
  text-align: left;
  color: #4f3d1a;
}

.suggestion-list li:hover {
  background-color: #fff8e1;
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

  .styled-input {
    font-size: 0.95rem;
  }
}

.feedback {
  margin-top: 1rem;
  font-size: 1rem;
  text-align: center;
}

.feedback.success {
  color: #2e7d32;
  /* green */
}

.feedback.error {
  color: #c62828;
  /* red */
}
</style>
