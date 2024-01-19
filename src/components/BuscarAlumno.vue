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
  
  <script>
  export default {
    data() {
      return {
        idBuscar: "",
        alumnoEncontrado: null,
      };
    },
    methods: {
      buscarAlumno() {
        const idBuscado = parseInt(this.idBuscar);
        this.alumnoEncontrado = this.alumnos.find((alumno) => alumno.id === idBuscado);
      },
    },
    watch: {
      alumnos() {
        this.alumnoEncontrado = null;
      },
    },
    props: {
      alumnos: Array,
    },
  };
  </script>
  