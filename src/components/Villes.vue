<template>
  <form>
    <div id="v-model-select" class="demo">
      <select v-model="selected">
        <option disabled value="">Sellectionnez une ville</option>
        <option>Paris</option>
        <option>Lyon</option>
        <option>Marseille</option>
      </select>
      <br>
      <br>
      <span>Vous avez sellectionné : {{ selected }}</span>
    </div>
  </form>
</template>

<script>
/* export default {
  setup() {
      return {
        selected: "",
      };
  }
}; */
 
export default {
data() {
    return {
      selected: "",

    };
  }
}
</script>

<style scoped>
.demo {
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