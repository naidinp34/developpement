<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="La Météo du Jour!"/>
  <Ville selectedFruit/>
  
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Ville from './components/Villes.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    Ville
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
