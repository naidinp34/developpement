<template>
  <div class="filters">

  <label for="basic-dropdown">Sellectionner une Ville: </label>
  <select name="basic-dropdown" v-model="selectedVilles">
    <option>Paris</option>
    <option>Lyon</option>
    <option>Marseille</option>
    <option>Strasbourg</option>
    <option>Montpellier</option>
    <option>Perpignan</option>
    <option>Rennes</option>
  </select>

    <div class="result">
      Vous avez selectionné : <strong>{{ selectedVilles }}</strong>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      selectedVilles: ''
    }
  }
}
</script>

<style scoped>

.filters {
  width: 800px;
  margin: 0 auto;
}

.filter {
  text-align: left;
}

.result {
  margin-top: 30px;
  text-align: left;
}

label {
  display: block;
}

</style>