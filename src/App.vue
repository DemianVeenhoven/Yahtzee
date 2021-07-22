<template>
  <div id="app">
    <yahtzee/>
  </div>
</template>

<script>
import Yahtzee from './components/Yahtzee.vue'

export default {
  name: 'App',
  components: {
    Yahtzee
  }
}
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
