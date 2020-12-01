<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>Prise en Main Vue3 - Projet :</h1>
  <HelloWorld msg="La Météo du Jour!"/>
  <donneesMeteo donneesMeteo />
  <Ville selectedVille/>
  
  
  
  
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import donneesMeteo from './components/Meteo.vue'
import Ville from './components/Villes.vue'

export default {
  
  components: {
    HelloWorld,
    donneesMeteo,
    Ville,
    
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
