<template>
  <div id="app">
    <AffordabilityCalculator msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import AffordabilityCalculator from "./components/AffordabilityCalculator.vue";

export default {
  name: "app",
  components: {
    AffordabilityCalculator
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 14px;
}
</style>
