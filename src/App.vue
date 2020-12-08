<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>Prise en Main Vue3 - Projet :</h1>
  <HelloWorld msg="Météo du Jour !"/>
  
  <Ville selectedVille/>
  <div>
  <Temp currentTemp />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Ville from './components/Villes.vue'
//import Temp from './components/Villes.vue'
//import Hum from './components/Villes.vue'
//import Vent from './components/Villes.vue'

export default {
  
  components: {
    HelloWorld,
    Ville,
    
  //  Hum,
  //  Vent,
   }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #4cb80d;
  margin-top: 60px;
}
</style>
