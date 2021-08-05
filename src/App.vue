<template>
  <Ricky></Ricky>
</template>

<script>
import Ricky from "./components/rickApp.vue";


export default {
  name: "App",
  components: {
    Ricky
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
