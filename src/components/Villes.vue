<template>
  <form>
    <div id="v-model-select" class="demo">
      <select v-model="selected" @change="onChange()">
        <option disabled value="">Sellectionnez une ville</option>
        <option>Paris</option>
        <option>Lyon</option>
        <option>Marseille</option>
        <option>Montpellier</option>
        <option>Strasbourg</option>
        <option>Bucarest</option>
      </select>
      <br />
      <br />
      <span>Vous avez sellectionné : {{ selected }}</span>
    </div>
    <br>
    <div>
      <span>Général : <v-text-field v-model="description">{{ description }} </v-text-field></span>
    </div>
    <br>
    <div>
      <span>Temperature : <v-text-field v-model="currentTemp">{{ currentTemp }} °C</v-text-field></span>
    </div>
     <br>
    <div>
      <span>Umidité : <v-text-field v-model="humidity">{{ humidity }} %</v-text-field></span>
    </div>
    <br>
    <div>
      <span>Vent : <v-text-field v-model="wind">{{ wind }} m/s</v-text-field></span>
    </div>
    <br>
    
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
import axios from "axios";

export default {
  data() {
    return {
      selected: "",
      infos: [],
      currentTemp: "",
      humidity: "",
      wind: "",
      description: "",
    };
  },
  methods: {
    onChange() {
      //this.currentTemp = "toto";
      
      
      var requestUrl =
        "http://api.openweathermap.org/data/2.5/weather?&units=metric&lang=fr&appid=4062dd1abc4e0122655b4f9bef3323a8&q=";

      var parametersString = this.selected;

      requestUrl = requestUrl + parametersString;

      axios
        .get(requestUrl)
        .then((response) => {
          this.currentTemp = response.data.main.temp;
          this.humidity = response.data.main.humidity;
          this.wind = response.data.wind.speed;
          this.description = response.data.weather[0].description;
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
        
      //axios.get(
      //    "http://api.openweathermap.org/data/2.5/weather?q=montpellier&units=metric&lang=fr&appid=4062dd1abc4e0122655b4f9bef3323a8"
      //  )
      //.then((response) => {
      //    console.warn(resp);
      //  this.infos = response.data;
      //})
      //.catch((error) => {
      //  console.log(error);
      //})
      this.loading = true;
    },
  },
  mounted() {},
};
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