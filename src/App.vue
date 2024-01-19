<template>
  <div>
    <AlumnosList />
  </div>
</template>

<script>
import AlumnosList from "@/components/ListaAlumnos.vue";

export default {
  components: {
    AlumnosList,
  },
};
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
