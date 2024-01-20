<template>
  <div>
    <h2>Lista de Alumnos</h2>
    <button @click="verTodos">Ver Todos</button>
    <button @click="mostrarAgregar">Agregar</button>
    <button @click="mostrarBuscar">Buscar</button>

    <AgregarAlumno v-if="mostrarComponente === 'agregar'" @agregarAlumno="insertarAlumno" />
    <BuscarAlumno v-if="mostrarComponente === 'buscar'" :alumnos="alumnos" />
    <ModificarAlumno v-if="mostrarComponente === 'modificar'" :alumnoSeleccionado="alumnoSeleccionado"
      @modificarAlumno="modificarAlumno" />

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

<script setup>
import { ref, onMounted } from 'vue';
import AgregarAlumno from "@/components/AgregarAlumno.vue";
import BuscarAlumno from "@/components/BuscarAlumno.vue";
import ModificarAlumno from "@/components/ModificarAlumno.vue";

const localStorageKey = 'alumnosData';

const alumnos = ref(JSON.parse(localStorage.getItem(localStorageKey)) || [
  { id: 1, nombre: "Markel", fechaNacimiento: "1990-05-15" },
  { id: 2, nombre: "Gaia", fechaNacimiento: "1995-08-20" },
]);

let alumnoSeleccionado = ref(null);
let mostrarComponente = ref(null);
let mostrarListaAlumnos = ref(true);

const guardarAlumnosEnLocalStorage = () => {
  localStorage.setItem(localStorageKey, JSON.stringify(alumnos.value));
};

onMounted(() => {
  guardarAlumnosEnLocalStorage();
});

const insertarAlumno = (alumno) => {
  alumnos.value.push(alumno);
  mostrarComponente.value = null;
  mostrarListaAlumnos.value = false;
  guardarAlumnosEnLocalStorage();
};

const verTodos = () => {
  alumnoSeleccionado.value = null;
  mostrarComponente.value = null;
  mostrarListaAlumnos.value = true;
};

const verAlumno = (id) => {
  alumnoSeleccionado.value = alumnos.value.find((alumno) => alumno.id === id);
  mostrarComponente.value = null;
  mostrarListaAlumnos.value = true;
};

const eliminarAlumno = (id) => {
  alumnos.value = alumnos.value.filter((alumno) => alumno.id !== id);
  alumnoSeleccionado.value = null;
  mostrarListaAlumnos.value = true;
  guardarAlumnosEnLocalStorage();
};

const mostrarAgregar = () => {
  mostrarComponente.value = 'agregar';
  alumnoSeleccionado.value = null;
  mostrarListaAlumnos.value = false;
};

const mostrarBuscar = () => {
  mostrarComponente.value = 'buscar';
  alumnoSeleccionado.value = null;
  mostrarListaAlumnos.value = false;
};

const mostrarModificar = () => {
  mostrarComponente.value = 'modificar';
};

const modificarAlumno = (alumnoModificado) => {
  const index = alumnos.value.findIndex((alumno) => alumno.id === alumnoModificado.id);
  if (index !== -1) {
    alumnos.value[index] = alumnoModificado;
    alumnoSeleccionado.value = alumnoModificado;
    mostrarComponente.value = null;
    mostrarListaAlumnos.value = true;
    guardarAlumnosEnLocalStorage();
  }
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

li {
  list-style: none;
}
</style>
