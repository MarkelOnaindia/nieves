<template>
  <div>
    <h2>Lista de Alumnos</h2>
    <button @click="verTodos">Ver Todos</button>
    <button @click="mostrarAgregar">Agregar</button>
    <button @click="mostrarBuscar">Buscar</button>

    <AgregarAlumno v-if="mostrarComponente === 'agregar'" @agregarAlumno="insertarAlumno" />
    <BuscarAlumno v-if="mostrarComponente === 'buscar'" :alumnos="alumnos" />
    <ModificarAlumno v-if="mostrarComponente === 'modificar'" :alumnoSeleccionado="alumnoSeleccionado" @modificarAlumno="modificarAlumno" />

    <ul v-if="mostrarListaAlumnos && alumnos.length > 0">
      <li v-for="alumno in alumnos" :key="alumno.id">
        <div class="alumno-info">
          <p>{{ alumno.nombre }} - {{ alumno.fechaNacimiento }}</p>
          <div class="botones-alumno">
            <button @click="verAlumno(alumno.id)">Ver Detalles</button>
            <button @click="eliminarAlumno(alumno.id)">Eliminar</button>
            <button @click="modificarAlumno(alumno)">Modificar</button>
          </div>
        </div>
      </li>
    </ul>

    <div v-if="alumnoSeleccionado">
      <h3>Detalles del Alumno</h3>
      <p>ID: {{ alumnoSeleccionado.id }}</p>
      <p>Nombre: {{ alumnoSeleccionado.nombre }}</p>
      <p>Fecha de Nacimiento: {{ alumnoSeleccionado.fechaNacimiento }}</p>
    </div>
  </div>
</template>

<script>
import AgregarAlumno from "@/components/AgregarAlumno.vue";
import BuscarAlumno from "@/components/BuscarAlumno.vue";
import ModificarAlumno from "@/components/ModificarAlumno.vue";

export default {
  data() {
    return {
      alumnos: [
        { id: 1, nombre: "Juan", fechaNacimiento: "1990-05-15" },
        { id: 2, nombre: "Ana", fechaNacimiento: "1995-08-20" },
      ],
      alumnoSeleccionado: null,
      mostrarComponente: null,
      mostrarListaAlumnos: true,
    };
  },
  methods: {
    insertarAlumno(alumno) {
      this.alumnos.push(alumno);
      this.mostrarComponente = null;
      this.mostrarListaAlumnos = false;
    },
    verTodos() {
      this.alumnoSeleccionado = null;
      this.mostrarComponente = null;
      this.mostrarListaAlumnos = true;
    },
    verAlumno(id) {
      this.alumnoSeleccionado = this.alumnos.find((alumno) => alumno.id === id);
      this.mostrarComponente = null;
      this.mostrarListaAlumnos = true;
    },
    eliminarAlumno(id) {
      this.alumnos = this.alumnos.filter((alumno) => alumno.id !== id);
      this.alumnoSeleccionado = null;
      this.mostrarListaAlumnos = true;
    },
    mostrarAgregar() {
      this.mostrarComponente = 'agregar';
      this.alumnoSeleccionado = null;
      this.mostrarListaAlumnos = false;
    },
    mostrarBuscar() {
      this.mostrarComponente = 'buscar';
      this.alumnoSeleccionado = null;
      this.mostrarListaAlumnos = false;
    },
    mostrarModificar() {
      this.mostrarComponente = 'modificar';
    },
    modificarAlumno(alumnoModificado) {
      const index = this.alumnos.findIndex((alumno) => alumno.id === alumnoModificado.id);
      if (index !== -1) {
        this.alumnos[index] = alumnoModificado;
        this.alumnoSeleccionado = alumnoModificado;
        this.mostrarComponente = null;
        this.mostrarListaAlumnos = true;
      }
    },
  },
  components: {
    AgregarAlumno,
    BuscarAlumno,
    ModificarAlumno,
  },
};
</script>

<style scoped>
  .alumno-info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }

  .botones-alumno {
    display: flex;
    gap: 5px;
  }

  li{
    list-style: none;
  }
</style>
