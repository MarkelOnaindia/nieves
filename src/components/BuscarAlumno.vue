<template>
  <div>
    <h3>Buscar Alumno</h3>
    <div>
      <input v-model="idBuscar" type="number" min="1" placeholder="ID del Alumno">
      <button @click="buscarAlumno">Buscar</button>
    </div>
    <table v-if="alumnoEncontrado">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Fecha de Nacimiento</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{{ alumnoEncontrado.id }}</td>
          <td>{{ alumnoEncontrado.nombre }}</td>
          <td>{{ alumnoEncontrado.fechaNacimiento }}</td>
        </tr>
      </tbody>
    </table>
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

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}
</style>