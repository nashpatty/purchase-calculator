<template>
  <div id="app">
    <PurchaseCalculator />
  </div>
</template>

<script>
import PurchaseCalculator from "./components/PurchaseCalculator.vue";

export default {
  name: "app",
  components: {
    PurchaseCalculator
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
