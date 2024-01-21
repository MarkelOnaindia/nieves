<template>
  <div>
    <AlumnosList />
  </div>
</template>

<script setup>
import AlumnosList from "@/components/ListaAlumnos.vue";
import { provide, ref } from 'vue';

const frase= ref("Los alumnos de Desarrollo de Aplicaciones Web");

provide("fraseAHijo",frase);
</script>

<style>
#app {
  text-align: center;
  margin-top: 60px;
}

button {
  margin: 5px;
}
</style>
