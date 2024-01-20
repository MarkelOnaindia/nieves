<template>
  <div>
      <button @click="verTodos">Ver Todos</button>
      <button @click="mostrarAgregar">Agregar</button>
      <button @click="mostrarBuscar">Buscar</button>

      <table v-if="mostrarListaAlumnos">
          <thead>
              <tr>
                  <th>Nombre</th>
                  <th>Acciones</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="alumno in alumnos" :key="alumno.id">
                  <td>{{ alumno.nombre }}</td>
                  <td>
                      <button @click="verAlumno(alumno.id)">Ver Detalles</button>
                      <button @click="eliminarAlumno(alumno.id)">Eliminar</button>
                      <button @click="modificarAlumno(alumno)">Modificar</button>
                  </td>
              </tr>
          </tbody>
      </table>

      <AgregarAlumno v-if="mostrarComponente === 'agregar'" @agregarAlumno="insertarAlumno" />
      <BuscarAlumno v-if="mostrarComponente === 'buscar'" :alumnos="alumnos" />
      <ModificarAlumno v-if="mostrarComponente === 'modificar'" :alumnoSeleccionado="alumnoSeleccionado"
          @modificarAlumno="modificarAlumno" />

      <div v-if="alumnoSeleccionado">
          <table>
              <thead>
                  <tr>
                      <th>ID</th>
                      <th>Nombre</th>
                      <th>Fecha de nacimiento</th>
                  </tr>
              </thead>
              <tbody>
                  <tr>
                      <td>{{ alumnoSeleccionado.id }}</td>
                      <td>{{ alumnoSeleccionado.nombre }}</td>
                      <td>{{ alumnoSeleccionado.fechaNacimiento }}</td>
                  </tr>
              </tbody>
          </table>
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
      { id: 1, nombre: "Juan", fechaNacimiento: "1990-05-15" },
      { id: 2, nombre: "Ana", fechaNacimiento: "1995-08-20" },
  ]);

  let alumnoSeleccionado = ref(null);
  let mostrarComponente = ref(null);
  let mostrarListaAlumnos = ref(false);

  const guardarAlumnosEnLocalStorage = () => {
      localStorage.setItem(localStorageKey, JSON.stringify(alumnos.value));
  };

  onMounted(() => {
      guardarAlumnosEnLocalStorage();
  });

  const insertarAlumno = (alumno) => {
      alumnos.value.push(alumno);
      mostrarComponente.value = null;
      mostrarListaAlumnos.value = true;
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
  };

  const eliminarAlumno = (id) => {
      alumnos.value = alumnos.value.filter((alumno) => alumno.id !== id);
      alumnoSeleccionado.value = null;
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
</style>