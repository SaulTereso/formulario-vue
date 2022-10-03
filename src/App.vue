<template>
  <!-- <Prueba/> -->
  <Registros/>
  <!-- <Login/> -->
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Prueba from './components/Prueba.vue'
import Login from './components/Login.vue'
import Registros from './components/Registros.vue'

export default {
  name: 'App',
  components: {
    //HelloWorld,
    Prueba,
    Login,
    Registros
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

/* //?Estilos de Login */
.title {
  text-align: center;
  color: #2c3e50;
}
.form {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  
  /* background: rgba(19, 35, 47, 0.9); */
  /* box-shadow: 0 4px 10px 4px rgba(0, 0, 0, 0.3); */
  border-radius: 5px;
  padding: 40px;
}

</style>
