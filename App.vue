<template>
  <div id="app">
    <SearchQuote/>
  </div>
</template>

<script>
import SearchQuote from "./components/SearchQuote.vue";
import TheQuote from "./components/TheQuote.vue";

export default {
  name: 'app',
  components: {
    SearchQuote,
 
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
