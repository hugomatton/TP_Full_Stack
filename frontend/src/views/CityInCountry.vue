<script setup>
  import {onMounted, onUpdated} from 'vue'
  import {reactive, ref} from 'vue'

  let data = reactive(
    {
      countries:[],
      villes:[]
    },
  )

  let id_selected_country = ref(0)
  
  
  function fetchCountry(){
    fetch("api/countries")
    .then((response)=>response.json())
    .then((dataJson)=> data.countries = dataJson._embedded.countries)
  }

  function fetchCities(event){
    let url = event.target.value
    fetch(url)
    .then((response)=>response.json())
    .then((dataJson)=>afficherCities(dataJson))
  }

  function afficherCities(dataJson){
    data.villes = dataJson._embedded.cities
  }

  onMounted(fetchCountry)

  function deleteCity(event){
    let url_ville = event.target.id
    fetch(url_ville, {method:'DELETE'})
    .then(response => {console.log("ville sup")})
  }

 
</script>

<template>
    <h1>Voir les villes de chaque pays</h1>
    <select @change="fetchCities" >
      <option :value="c._links.cities.href" v-for="(c, index) in data.countries" >
        {{c.name}}
      </option>
    </select>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Population</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(v,index) in data.villes" :key=[index]>
          <td>{{v.name}}</td>
          <td>{{v.population}}</td>
          <td><button :id="v._links.city.href" @click="deleteCity">Delete</button></td>
        </tr>
      </tbody>
    </table>
    
</template>

<style>

</style>