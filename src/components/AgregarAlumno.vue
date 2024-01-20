<template>
  <div>
    <h3>Agregar Alumno</h3>
    <form @submit.prevent="agregarAlumno">
      <table>
        <tr>
          <td>ID:</td>
          <td><input v-model="nuevoAlumno.id" type="number" required min="1"></td>
        </tr>
        <tr>
          <td>Nombre:</td>
          <td><input v-model="nuevoAlumno.nombre" type="text" required></td>
        </tr>
        <tr>
          <td>Fecha de Nacimiento:</td>
          <td><input v-model="nuevoAlumno.fechaNacimiento" type="date" required></td>
        </tr>
      </table>
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