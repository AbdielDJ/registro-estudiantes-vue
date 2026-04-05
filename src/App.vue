<script setup>
import { ref } from 'vue'

const nombre = ref('')
const apellido = ref('')
const genero = ref('')
const carrera = ref('')
const estudiantes = ref([])
const animating = ref(false)

function agregarEstudiante() {
  if (!nombre.value || !apellido.value || !genero.value || !carrera.value) return
  
  animating.value = true
  estudiantes.value.push({
    nombre: nombre.value,
    apellido: apellido.value,
    genero: genero.value,
    carrera: carrera.value
  })

  nombre.value = ''
  apellido.value = ''
  genero.value = ''
  carrera.value = ''

  setTimeout(() => animating.value = false, 400)
}
</script>

<template>
  <div class="app">
    <div class="bg-orbs">
      <div class="orb orb1"></div>
      <div class="orb orb2"></div>
      <div class="orb orb3"></div>
    </div>

    <div class="wrapper">
      <header class="header">
        <div class="header-badge">Vue.js + Vite</div>
        <h1 class="title">Registro de <span>Estudiantes</span></h1>
        <p class="subtitle">Formulario interactivo con visualización dinámica</p>
      </header>

      <div class="card form-card">
        <div class="card-label">Nuevo Registro</div>

        <div class="form-grid">
          <div class="field">
            <label>Nombre</label>
            <input v-model="nombre" type="text" placeholder="Ej: Juan" />
          </div>
          <div class="field">
            <label>Apellido</label>
            <input v-model="apellido" type="text" placeholder="Ej: Pérez" />
          </div>
        </div>

        <div class="field">
          <label>Género</label>
          <div class="radio-group">
            <label class="radio-option" :class="{ active: genero === 'Masculino' }">
              <input type="radio" v-model="genero" value="Masculino" hidden />
              <span class="radio-icon">♂</span> Masculino
            </label>
            <label class="radio-option" :class="{ active: genero === 'Femenino' }">
              <input type="radio" v-model="genero" value="Femenino" hidden />
              <span class="radio-icon">♀</span> Femenino
            </label>
            <label class="radio-option" :class="{ active: genero === 'Otro' }">
              <input type="radio" v-model="genero" value="Otro" hidden />
              <span class="radio-icon">◈</span> Otro
            </label>
          </div>
        </div>

        <div class="field">
          <label>Carrera</label>
          <select v-model="carrera">
            <option value="">— Selecciona tu carrera —</option>
            <option value="Ingeniería en Sistemas">Ingeniería en Sistemas</option>
            <option value="Administración">Administración</option>
            <option value="Contabilidad">Contabilidad</option>
            <option value="Derecho">Derecho</option>
            <option value="Medicina">Medicina</option>
          </select>
        </div>

        <button class="btn-agregar" @click="agregarEstudiante" :class="{ pulse: animating }">
          <span>+</span> Agregar Estudiante
        </button>
      </div>

      <div class="card table-card" v-if="estudiantes.length > 0">
        <div class="card-label">Registros — <strong>{{ estudiantes.length }}</strong> estudiante{{ estudiantes.length > 1 ? 's' : '' }}</div>
        <div class="table-wrap">
          <table>
            <thead>
              <tr>
                <th>#</th>
                <th>Nombre</th>
                <th>Apellido</th>
                <th>Género</th>
                <th>Carrera</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(e, i) in estudiantes" :key="i" class="row-fade">
                <td class="num">{{ i + 1 }}</td>
                <td>{{ e.nombre }}</td>
                <td>{{ e.apellido }}</td>
                <td>
                  <span class="badge" :class="e.genero.toLowerCase()">{{ e.genero }}</span>
                </td>
                <td>{{ e.carrera }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="empty" v-else>
        <div class="empty-icon">🎓</div>
        <p>Aún no hay registros. ¡Agrega el primero!</p>
      </div>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'DM Sans', sans-serif;
  background: #0a0a1a;
  min-height: 100vh;
  color: #e0e0f0;
}

.app {
  min-height: 100vh;
  position: relative;
  overflow: hidden;
}

/* Orbs de fondo */
.bg-orbs { position: fixed; inset: 0; pointer-events: none; z-index: 0; }
.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.35;
  animation: float 10s ease-in-out infinite;
}
.orb1 { width: 500px; height: 500px; background: #6c63ff; top: -100px; left: -100px; animation-delay: 0s; }
.orb2 { width: 400px; height: 400px; background: #00d4ff; bottom: -80px; right: -80px; animation-delay: -4s; }
.orb3 { width: 300px; height: 300px; background: #ff6b9d; top: 50%; left: 50%; transform: translate(-50%,-50%); animation-delay: -7s; }

@keyframes float {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-30px) scale(1.05); }
}

.wrapper {
  position: relative;
  z-index: 1;
  max-width: 760px;
  margin: 0 auto;
  padding: 48px 20px 60px;
}

/* Header */
.header { text-align: center; margin-bottom: 40px; }
.header-badge {
  display: inline-block;
  background: rgba(108, 99, 255, 0.2);
  border: 1px solid rgba(108, 99, 255, 0.5);
  color: #a59fff;
  font-size: 12px;
  font-family: 'Syne', sans-serif;
  letter-spacing: 2px;
  text-transform: uppercase;
  padding: 6px 16px;
  border-radius: 20px;
  margin-bottom: 16px;
}
.title {
  font-family: 'Syne', sans-serif;
  font-size: clamp(32px, 6vw, 52px);
  color: #ffffff;
  margin-bottom: 10px;
  padding: 15px;
}
.title span {
  background: linear-gradient(135deg, #6c63ff, #00d4ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.subtitle { font-size: 15px; color: rgba(255,255,255,0.4); font-weight: 300; }

/* Cards glassmorphism */
.card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  padding: 32px;
  margin-bottom: 24px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.card-label {
  font-family: 'Syne', sans-serif;
  font-size: 11px;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: rgba(255,255,255,0.35);
  margin-bottom: 24px;
}
.card-label strong { color: #6c63ff; }

/* Formulario */
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
@media (max-width: 500px) { .form-grid { grid-template-columns: 1fr; } }

.field { margin-bottom: 20px; }
.field label {
  display: block;
  font-size: 12px;
  font-family: 'Syne', sans-serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: rgba(255,255,255,0.5);
  margin-bottom: 8px;
}

input[type="text"], select {
  width: 100%;
  padding: 12px 16px;
  background: rgba(255,255,255,0.07);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 12px;
  color: #ffffff;
  font-family: 'DM Sans', sans-serif;
  font-size: 14px;
  outline: none;
  transition: all 0.25s;
}
input[type="text"]:focus, select:focus {
  border-color: #6c63ff;
  background: rgba(108, 99, 255, 0.1);
  box-shadow: 0 0 0 3px rgba(108, 99, 255, 0.15);
}
input::placeholder { color: rgba(255,255,255,0.2); }
select option { background: #1a1a2e; color: #fff; }

/* Radio buttons personalizados */
.radio-group { display: flex; gap: 12px; flex-wrap: wrap; }
.radio-option {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 18px;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 10px;
  cursor: pointer;
  font-size: 13px;
  color: rgba(255,255,255,0.6);
  transition: all 0.2s;
  user-select: none;
}
.radio-option:hover { border-color: rgba(108,99,255,0.4); color: white; }
.radio-option.active {
  background: rgba(108, 99, 255, 0.2);
  border-color: #6c63ff;
  color: #ffffff;
  box-shadow: 0 0 12px rgba(108,99,255,0.25);
}
.radio-icon { font-size: 16px; }

/* Botón */
.btn-agregar {
  width: 100%;
  padding: 14px;
  margin-top: 8px;
  background: linear-gradient(135deg, #6c63ff, #00d4ff);
  border: none;
  border-radius: 12px;
  color: white;
  font-family: 'Syne', sans-serif;
  font-size: 15px;
  font-weight: 700;
  letter-spacing: 1px;
  cursor: pointer;
  transition: all 0.25s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}
.btn-agregar span { font-size: 20px; line-height: 1; }
.btn-agregar:hover { transform: translateY(-2px); box-shadow: 0 8px 25px rgba(108,99,255,0.4); }
.btn-agregar:active { transform: translateY(0); }
.btn-agregar.pulse { animation: btnPulse 0.4s ease; }
@keyframes btnPulse {
  0% { transform: scale(1); }
  50% { transform: scale(0.97); }
  100% { transform: scale(1); }
}

/* Tabla */
.table-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; }
thead tr {
  border-bottom: 1px solid rgba(255,255,255,0.08);
}
th {
  font-family: 'Syne', sans-serif;
  font-size: 11px;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: rgba(255,255,255,0.3);
  padding: 10px 14px;
  text-align: left;
}
td {
  padding: 14px;
  font-size: 14px;
  color: rgba(255,255,255,0.8);
  border-bottom: 1px solid rgba(255,255,255,0.05);
}
tbody tr {
  transition: background 0.2s;
}
tbody tr:hover { background: rgba(255,255,255,0.04); }
tbody tr:last-child td { border-bottom: none; }

.num {
  font-family: 'Syne', sans-serif;
  font-weight: 700;
  color: rgba(108,99,255,0.7);
  font-size: 13px;
}

/* Badges de género */
.badge {
  display: inline-block;
  padding: 3px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 500;
}
.badge.masculino { background: rgba(0, 212, 255, 0.15); color: #00d4ff; border: 1px solid rgba(0,212,255,0.3); }
.badge.femenino { background: rgba(255, 107, 157, 0.15); color: #ff6b9d; border: 1px solid rgba(255,107,157,0.3); }
.badge.otro { background: rgba(108, 99, 255, 0.15); color: #a59fff; border: 1px solid rgba(108,99,255,0.3); }

/* Row animation */
.row-fade { animation: rowIn 0.3s ease; }
@keyframes rowIn {
  from { opacity: 0; transform: translateX(-10px); }
  to { opacity: 1; transform: translateX(0); }
}

/* Empty state */
.empty {
  text-align: center;
  padding: 40px;
  color: rgba(255,255,255,0.25);
}
.empty-icon { font-size: 48px; margin-bottom: 12px; }
.empty p { font-size: 14px; }
</style>