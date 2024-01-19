<template>
    <div>
      <h3>Agregar Alumno</h3>
      <form @submit.prevent="agregarAlumno">
        <label>ID: <input v-model="nuevoAlumno.id" type="text" required></label><br>
        <label>Nombre: <input v-model="nuevoAlumno.nombre" type="text" required></label><br>
        <label>Fecha de Nacimiento: <input v-model="nuevoAlumno.fechaNacimiento" type="date" required></label><br>
        <button type="submit">Agregar</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        nuevoAlumno: {
          id: '',
          nombre: '',
          fechaNacimiento: '',
        },
      };
    },
    methods: {
      agregarAlumno() {
        this.$emit('agregarAlumno', this.nuevoAlumno);
        this.nuevoAlumno = {
          id: '',
          nombre: '',
          fechaNacimiento: '',
        };
      },
    },
  };
  </script>
  