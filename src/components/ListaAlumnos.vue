<template>
  <div>
   <h2>Lista de Alumnos</h2>
    <button @click="verTodos">Ver Todos</button>
    <button @click="mostrarAgregar">Agregar</button>
    <button @click="mostrarBuscar">Buscar</button>

    <template v-if="mostrarComponente === 'verTodos' || !mostrarComponente">
      <table>
        <table>
          <thead>
            <tr>
              <th>Nombre</th>
              <th>Fecha de Nacimiento</th>
              <th>Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="alumno in alumnos" :key="alumno.id">
              <td>
                <span v-if="alumno.id !== alumnoModificandoId">{{ alumno.nombre }}</span>
                <input v-else v-model="alumnoModificado.nombre" type="text" required>
              </td>
              <td>
                <span v-if="alumno.id !== alumnoModificandoId">{{ alumno.fechaNacimiento }}</span>
                <input v-else v-model="alumnoModificado.fechaNacimiento" type="date" required>
              </td>
              <td>
                <button @click="verAlumno(alumno.id)">Ver Detalles</button>
                <button @click="eliminarAlumno(alumno.id)">Eliminar</button>
                <button @click="iniciarModificacion(alumno.id)">Modificar</button>
                <button v-if="alumno.id === alumnoModificandoId" @click="modificarAlumno" class="guardar">Guardar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </table>
    </template>

    <template v-if="mostrarComponente === 'agregar'">
      <AgregarAlumno @agregarAlumno="insertarAlumno" />
    </template>

    <template v-if="mostrarComponente === 'buscar'">
      <BuscarAlumno :alumnos="alumnos" />
    </template>
  </div>
</template>

<script setup>
import { ref, onMounted, inject } from 'vue';
import AgregarAlumno from "@/components/AgregarAlumno.vue";
import BuscarAlumno from "@/components/BuscarAlumno.vue";

const localStorageKey = 'alumnosData';

const alumnos = ref(JSON.parse(localStorage.getItem(localStorageKey)) || [
  { id: 1, nombre: "Juan", fechaNacimiento: "1990-05-15" },
  { id: 2, nombre: "Ana", fechaNacimiento: "1995-08-20" },
]);

let alumnoModificandoId = ref(null);
let alumnoModificado = ref({ id: null, nombre: '', fechaNacimiento: '' });
let mostrarComponente = ref(null);

const guardarAlumnosEnLocalStorage = () => {
  localStorage.setItem(localStorageKey, JSON.stringify(alumnos.value));
};

onMounted(() => {
  guardarAlumnosEnLocalStorage();
});

const insertarAlumno = (alumno) => {
  alumnos.value.push(alumno);
  mostrarComponente.value = null;
  guardarAlumnosEnLocalStorage();
};

const verTodos = () => {
  alumnoModificandoId.value = null;
  alumnoModificado.value = { id: null, nombre: '', fechaNacimiento: '' };
  mostrarComponente.value = null;
};

const iniciarModificacion = (id) => {
  alumnoModificandoId.value = id;
  const alumno = alumnos.value.find((a) => a.id === id);
  alumnoModificado.value = { ...alumno };
};

const modificarAlumno = () => {
  const index = alumnos.value.findIndex((alumno) => alumno.id === alumnoModificandoId.value);
  if (index !== -1) {
    alumnos.value[index] = alumnoModificado.value;
    guardarAlumnosEnLocalStorage();
    alumnoModificandoId.value = null;
    alumnoModificado.value = { id: null, nombre: '', fechaNacimiento: '' };
  }
};

const verAlumno = (id) => {
  const alumno = alumnos.value.find((a) => a.id === id);
  alert(`Detalles del alumno:\nID: ${alumno.id}\nNombre: ${alumno.nombre}\nFecha de Nacimiento: ${alumno.fechaNacimiento}`);
};

const eliminarAlumno = (id) => {
  if (confirm("¿Estás seguro de que deseas eliminar este alumno?")) {
    alumnos.value = alumnos.value.filter((alumno) => alumno.id !== id);
    guardarAlumnosEnLocalStorage();
  }
};

const mostrarAgregar = () => {
  mostrarComponente.value = 'agregar';
};

const mostrarBuscar = () => {
  mostrarComponente.value = 'buscar';
};
</script>


<style scoped>
h2 {
  margin-bottom: 10px;
}

button {
  margin-right: 10px;
  margin-bottom: 10px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

input {
  width: 80%;
  padding: 5px;
}

.guardar {
  background-color: green;
  color: white;
}
</style>