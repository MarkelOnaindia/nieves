<template>
  <div>
    <h3>Buscar Alumno</h3>
    <div>
      <input v-model="idBuscar" type="text" placeholder="ID del Alumno">
      <button @click="buscarAlumno">Buscar</button>
    </div>
    <div v-if="alumnoEncontrado">
      <h3>Detalles del Alumno</h3>
      <p>ID: {{ alumnoEncontrado.id }}</p>
      <p>Nombre: {{ alumnoEncontrado.nombre }}</p>
      <p>Fecha de Nacimiento: {{ alumnoEncontrado.fechaNacimiento }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps(['alumnos']);
const idBuscar = ref('');
const alumnoEncontrado = ref(null);

const buscarAlumno = () => {
  const idBuscado = parseInt(idBuscar.value);
  alumnoEncontrado.value = props.alumnos.find((alumno) => alumno.id === idBuscado);
};

watch(() => props.alumnos, () => {
  alumnoEncontrado.value = null;
});

</script>
