<template>
  <div id="app">
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 100%;
  min-height: 100vh;
}
</style>
