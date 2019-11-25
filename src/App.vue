<template>
  <div id="app">
    <Frog/>
    <Frog v-bind:boxes="7" v-bind:selected="4"/>
  </div>
</template>

<script>
import Frog from './components/Frog.vue'

export default {
  name: 'app',
  components: {
    Frog
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
