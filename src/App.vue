<template>
   <div id="app" :class="typeof weather.main != 'undefined'
   && weather.main.temp>24 && weather.main.temp<32?'warm':''
  ">
      <main>
        <div class="search-box">
          <input type="text" name="" id=""
           class="search-bar" 
           placeholder="Search..."
            v-model="query" @keypress="fetchWeather"/>
        </div>
    
        <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
          <div class="location-box">
            <div class="location">{{weather.name}},{{weather.sys.country}}</div>
            <div class="date">{{dateBuilder()}}</div>
          </div>
          <div class="weather-box">
            <div class="temp">{{Math.round(weather.main.temp)}}℃</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </main>
   </div>
</template>

<script>
//import { json } from 'body-parser'

export default {
  name: 'App',
  data(){
    return{
      api_key:'160087966769be075006de54007badfa',
      url_base:"https://api.openweathermap.org/data/2.5/",
      query:'',
      weather:{}
    }
    
  },
  methods:{
    fetchWeather(e){
      if(e.key=="Enter"){
        console.log("helo");
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
         .then(res=>{
              return res.json();
        }).then(this.setResults);
      }
      
    },
    setResults(results){
      this.weather=results;
    },
    dateBuilder(){
      let d=new Date();
      let months=["January", "Febuary", "March", "April", "May", "June",
      "July", "August", "September", "October", "November", "December"];
      let days=["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }

  },
}
</script>

<style>
@import './components/app.css';

</style>
