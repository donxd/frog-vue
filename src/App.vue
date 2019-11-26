<template>
  <div id="app">
    <Frog/>
    <Frog v-bind:boxes="7" v-bind:selected="4"/>
    <!-- <HelloWorld msg="asfdasdf" /> -->
  </div>
</template>

<script>
import Frog from './components/Frog.vue'
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    Frog
    // HelloWorld
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
