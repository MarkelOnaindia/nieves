<template>
  <div>
    <h3>Agregar Alumno</h3>
    <form @submit.prevent="agregarAlumno">
      <label>ID: <input v-model="nuevoAlumno.id" type="number" required min="1"></label><br>
      <label>Nombre: <input v-model="nuevoAlumno.nombre" type="text" required></label><br>
      <label>Fecha de Nacimiento: <input v-model="nuevoAlumno.fechaNacimiento" type="date" required></label><br>
      <button type="submit">Agregar</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['agregarAlumno']);

const nuevoAlumno = ref({
  id: '',
  nombre: '',
  fechaNacimiento: '',
});

const agregarAlumno = () => {
  if (validarIdUnico(nuevoAlumno.value.id)) {
    emit('agregarAlumno', nuevoAlumno.value);
    nuevoAlumno.value = {
      id: '',
      nombre: '',
      fechaNacimiento: '',
    };
  } else {
    alert('El ID ya existe. Elija otro ID único.');
  }
};

const validarIdUnico = (nuevoId) => {
  const alumnosGuardados = JSON.parse(localStorage.getItem('alumnosData')) || [];

  return !alumnosGuardados.some((alumno) => alumno.id === nuevoId);
};
</script>
