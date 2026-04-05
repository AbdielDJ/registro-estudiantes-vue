<script setup>
import { ref } from 'vue'

// Datos del formulario
const nombre = ref('')
const apellido = ref('')
const genero = ref('')
const carrera = ref('')

// Lista donde guardaremos los registros
const estudiantes = ref([])

// Función que se ejecuta al enviar el formulario
function agregarEstudiante() {
  estudiantes.value.push({
    nombre: nombre.value,
    apellido: apellido.value,
    genero: genero.value,
    carrera: carrera.value
  })

  // Limpiar el formulario después de agregar
  nombre.value = ''
  apellido.value = ''
  genero.value = ''
  carrera.value = ''
}
</script>

<template>
  <div class="contenedor">
    <h1>Registro de Estudiantes</h1>

    <!-- FORMULARIO -->
    <div class="formulario">
      <div class="campo">
        <label>Nombre:</label>
        <input v-model="nombre" type="text" placeholder="Escribe tu nombre" />
      </div>

      <div class="campo">
        <label>Apellido:</label>
        <input v-model="apellido" type="text" placeholder="Escribe tu apellido" />
      </div>

      <div class="campo">
        <label>Género:</label>
        <label><input type="radio" v-model="genero" value="Masculino" /> Masculino</label>
        <label><input type="radio" v-model="genero" value="Femenino" /> Femenino</label>
        <label><input type="radio" v-model="genero" value="Otro" /> Otro</label>
      </div>

      <div class="campo">
        <label>Carrera:</label>
        <select v-model="carrera">
          <option value="">-- Selecciona --</option>
          <option value="Ingeniería en Sistemas">Ingeniería en Sistemas</option>
          <option value="Administración">Administración</option>
          <option value="Contabilidad">Contabilidad</option>
          <option value="Derecho">Derecho</option>
          <option value="Medicina">Medicina</option>
        </select>
      </div>

      <button @click="agregarEstudiante">Agregar</button>
    </div>

    <!-- TABLA -->
    <table v-if="estudiantes.length > 0">
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
        <tr v-for="(estudiante, index) in estudiantes" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ estudiante.nombre }}</td>
          <td>{{ estudiante.apellido }}</td>
          <td>{{ estudiante.genero }}</td>
          <td>{{ estudiante.carrera }}</td>
        </tr>
      </tbody>
    </table>

    <p v-else>Aún no hay registros. ¡Agrega el primero!</p>
  </div>
</template>

<style>
* {
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  background-color: #f0f2f5;
  margin: 0;
  padding: 20px;
}

.contenedor {
  max-width: 750px;
  margin: 0 auto;
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  color: #2c3e50;
}

.formulario {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 30px;
}

.campo {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.campo label {
  font-weight: bold;
  color: #333;
}

input[type="text"], select {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
}

button {
  padding: 10px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background-color: #2980b9;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

th {
  background-color: #3498db;
  color: white;
}

tr:nth-child(even) {
  background-color: #f9f9f9;
}
</style>