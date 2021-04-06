<template>
  <div id="app">
    <CocktailSearch />
  </div>
</template>

<script>
import CocktailSearch from './components/CocktailSearch.vue'

export default {
  name: 'App',
  components: {
    CocktailSearch
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}
</style>
