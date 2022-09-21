<template>
  <!--HelloWorld/-->
  <Prueba/>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Prueba from './components/Prueba.vue'

export default {
  name: 'App',
  components: {
    //HelloWorld,
    Prueba
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
  /*background-color: #ffe0e9;*/
  min-height: 100vh;
}
</style>
