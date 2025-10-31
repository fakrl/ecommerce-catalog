<template>
  <div id="app">
    <h1>E-Commerce Catalog</h1>
    <ProductDisplay />
  </div>
</template>

<script>
import ProductDisplay from "./components/ProductDisplay.vue";

export default {
  name: "App",
  components: {
    ProductDisplay,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}

h1 {
  margin: 30px 0;
  color: #2c3e50;
}
</style>
