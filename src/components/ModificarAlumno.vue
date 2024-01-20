<template>
  <div>
    <h3>Modificar Alumno</h3>
    <label>Nombre: <input v-model="alumnoModificado.nombre" type="text" required></label><br>
    <label>Fecha de Nacimiento: <input v-model="alumnoModificado.fechaNacimiento" type="date" required></label><br>
    <button @click="modificarAlumno">Modificar</button>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps(['alumnoSeleccionado']);
const emit = defineEmits(['modificarAlumno']);

const alumnoModificado = ref({
  id: props.alumnoSeleccionado.id,
  nombre: props.alumnoSeleccionado.nombre,
  fechaNacimiento: props.alumnoSeleccionado.fechaNacimiento,
});

const modificarAlumno = () => {
  emit('modificarAlumno', alumnoModificado.value);
};
</script>

