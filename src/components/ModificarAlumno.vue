<template>
    <div>
      <h3>Modificar Alumno</h3>
      <form @submit.prevent="modificarAlumno">
        <label>ID: {{ alumnoSeleccionado.id }}</label><br>
        <label>Nombre: <input v-model="alumnoSeleccionado.nombre" type="text" required></label><br>
        <label>Fecha de Nacimiento: <input v-model="alumnoSeleccionado.fechaNacimiento" type="date" required></label><br>
        <button type="submit">Guardar Cambios</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      alumnoSeleccionado: Object,
    },
    methods: {
      modificarAlumno() {
        this.$emit('modificarAlumno', this.alumnoSeleccionado);
      },
    },
  };
  </script>
  