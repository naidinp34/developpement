<template>
    <div>
        <h1>Données Meteo pour la ville : </h1>
        
    </div>
    
</template>


<script>

//import {ville} from '@/components/Ville'
//import Vue from 'vue';
import axios from 'axios'
//import VueAxios from 'vue-axios'
//Vue.use(VueAxios,axios)

export default {
    name:"donneesMeteo",
    data(){
        return{
            infos:[]
        }
    },
    created()
    {
        axios.get('http://api.openweathermap.org/data/2.5/weather?q=montpellier&units=metric&lang=fr&appid=4062dd1abc4e0122655b4f9bef3323a8')
        .then((response)=>{
            //console.warn(resp);
        this.infos = response.data
        })
        .catch(error=> {
            console.log(error)
        })
    }
}
</script>