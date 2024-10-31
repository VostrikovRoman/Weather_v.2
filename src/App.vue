<template>
  <div class="background">
    <input id="text" type="text" value="Москва">
    <div v-if="!city_data">
      <p  class="load"><br> Загрузка...</p> 
      <p id="lon" class="hidden_object"></p>
      <p id="lat" class="hidden_object">sd</p>
    </div>
    
    <div v-else>
      <h1 class="title">Погода в городе: {{ city_data.response.GeoObjectCollection.featureMember[0].GeoObject.Point.pos }}</h1>
      <p id="lon" class="hidden_object">{{ city_data.response.GeoObjectCollection.featureMember[0].GeoObject.Point.pos.split(" ")[0] }}</p>
      <p id="lat" class="hidden_object">{{ city_data.response.GeoObjectCollection.featureMember[0].GeoObject.Point.pos.split(" ")[1] }}</p>
    </div>

    <!-- <div v-if="!city_data">
      <p id="lon" class="hidden_object">{{ city_data.response.GeoObjectCollection.featureMember[0].GeoObject.Point.pos.split(" ")[0] }}</p>
      <p id="lat" class="hidden_object">{{ city_data.response.GeoObjectCollection.featureMember[0].GeoObject.Point.pos.split(" ")[1] }}</p>
    </div> -->
    
    <p v-if="!weather_data" class="load"><br>Загрузка...</p> 
    
    
    <div class="weather" v-else>
      <div class="temp_box">
        <img class="image_weather" src="../src/assets/sun.png">
        <p class="temp">{{ weather_data.main.temp }} &deg;C</p> 
      </div>
      <div class="description">
        <p class="title title_p">{{ weather_data.weather[0].description.replace(weather_data.weather[0].description[0], weather_data.weather[0].description[0].toUpperCase()) }}</p>
        <p class="title title_p">Ощущается как {{ weather_data.main.feels_like }} &deg;C</p>
      </div>
      <div class="others_2">
        <div>
          <img class="title_o_2 image_others" src="../src/assets/barometer.png">
          <p class="title title_o_2">{{ weather_data.main.pressure }} мм рт.ст.</p>
        </div>
        <div>
          <img class="title_o_2 image_others" src="../src/assets/humidity.png">
          <p class="title title_o_2">{{ weather_data.main.humidity }} %</p>
        </div>
      </div>
      <div class="others">
        <p class="title title_o">MIN температура: {{ weather_data.main.temp_min }}</p>
        <p class="title title_o">MAX температура: {{ weather_data.main.temp_max }}</p>
        <p class="title title_o">Уровень моря: {{ weather_data.main.sea_level }}</p>
        <p class="title title_o">Уровень земли: {{ weather_data.main.grnd_level }}</p>
      </div>
      
    </div>
  </div>
  
</template>

<script>
// import { jsx } from 'vue/jsx-runtime';
// let city = "Москва";


export default {
  name: 'App',
  data(){
    return{
      weather_data:null,
      city_data:null
    }
  },
  mounted(){
    fetch(`https://geocode-maps.yandex.ru/1.x/?apikey=b9c5003e-d838-447d-87c3-3f021e76b867&geocode=${document.getElementById('text').value}&format=json`)
    .then(resp=>resp.json())
    .then(json=>{
      this.city_data=json;
    });

    
    fetch(`https://api.openweathermap.org/data/2.5/weather?lat=12&lon=12&appid=a06d5389c363dc0143a775466aef9cb3&units=metric&lang=ru`)
    .then(resp=>resp.json())
    .then(json=>{
      this.weather_data=json;
    });
    
    alert(document.getElementById('lat').textContent);
    
  },
  components: {
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #d8d8d8;
}
.hidden_object{
  display: block;
}
.weather{
  padding-left: 25px;
}
.background{
  background: linear-gradient(270deg, #3a50ab, #0b1529);
  border-radius: 10px;
  margin:40px 20px 20px 20px;
  padding-top: 10px;
  box-shadow: black 0px 0px 10px 0px;
}
.title{
  margin-left: 30px;
  font-size: 35px;
}
.temp{
  font-size: 80px;
  display: inline-block;
}
.image_weather{
  width: 70px;
  display: inline-block;
  margin-right: 25px;
}
.temp_box{
  width: 25%;
  display: inline-block;
}
.description{
  display: inline-block;
}
.title_o{
  display: inline-block;
  margin-right: 100px;
}
.title_o_2{
  display: inline-block;
  font-size: 40px;
}
.others_2{
  display: inline-block;
  margin-left: 355px;
}
.image_others{
  width: 55px;
}
.load{
  font-size: 25px;
  margin: 30px;
}
.others{
  border: #d8d8d8 solid 2px;
  border-bottom: none;
  border-radius: 10px;
  margin-right: 30px;
  margin-bottom: 30px;
}
</style>
