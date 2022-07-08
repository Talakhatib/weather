<template>
  <div class="home-img">
  <input type="text" placeholder="what you are looking for?" v-on:change="gettingweather" v-model="country"  />
  <div class="weather-description">
    <div v-if="show">
      <h2>Lebanon</h2>
      <h3>Beirut</h3>
      <h5>{{ location.localtime }}</h5>
      <div>
       <h2>{{ description }}</h2>
       <img :src="img" alt="weather of beirut">
       <h1>{{ current.temperature}} C</h1>
       <p>wind-degree: {{ current.wind_degree }}</p>
       <p>wind-speed: {{ current.wind_speed }}</p>
      </div>
    </div>
    <div v-else>
    <h2>{{ weatherlocation.country }}</h2>
    <h3>{{ weatherlocation.name }}</h3>
    <h5>{{ weatherlocation.localtime }}</h5>
     <div>
      <h2>{{ weatherdescription }}</h2>
       <img :src=" weatherimg" alt="weather of country">
        <h1>{{ weathercurrent.temperature }} C</h1>
        <p>wind-degree: {{ weathercurrent.wind_degree }}</p>
        <p>wind-speed: {{ weathercurrent.wind_speed }}</p>
     </div>
   </div>
  </div>
 </div>
</template>

<script>
// @ is an alias to /src

import { ref } from 'vue'
import env from '@/env.js'


export default {

  data(){
    return{

     location: [],
     current: [],
     img: "",
     description:""
    }
  },

  setup(){
   const country = ref("")
   const weathercurrent = ref([])
   const weatherlocation = ref([])
   const weatherimg = ref("")
   const weatherdescription = ref("")
   const show = ref(true)

     const gettingweather= () =>{
       if(country.value != ""){

         fetch('http://api.weatherstack.com/current?access_key='+ env.apikey +'&query='+ country.value)
         .then(response => response.json())
         .then(data =>{

              show.value = false
              weathercurrent.value = data.current
              weatherlocation.value = data.location
              weatherimg.value = data.current.weather_icons[0]
              weatherdescription.value = data.current.weather_descriptions[0]
              country.value = ""

         })
       }
     }

    return{ show,country , weathercurrent, weatherlocation, weatherimg, weatherdescription, gettingweather}
  },
  mounted(){
    fetch('http://api.weatherstack.com/current?access_key='+ env.apikey +'&query=beirut')
    .then(response => response.json())
    .then(data =>{

    this.current= data.current
    this.location= data.location
    this.img = data.current.weather_icons[0]
    this.description = data.current.weather_descriptions[0]

    })
  }


}
</script>

<style lang="scss">
.home-img{

background-image: url('https://cdn.wallpapersafari.com/27/13/KCqWxQ.jpg');
background-position: center;
background-size:cover;
input[type=text] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  border: none;
  background-color: #e6e6e6;
}

.weather-description{

margin: 25px 400px 75px 400px;
background:rgba(0,0,0,0.3);
padding-top:30px;
padding-bottom:30px;
text-align: center;
color: white;
font-weight: 600;
font-size: 22px;

}
}
</style>
