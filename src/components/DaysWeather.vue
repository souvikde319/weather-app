<template>
 <div class="days-tab text-center">
    <div class="loading" v-if="loading">loading..</div>
    
     
<div class="container" v-else>
 <div class="card-columns">
    <div class="card bg-primary" v-for="day in forecast" :key="day.date">
      <div class="card-body text-center">
        <p class="card-text">{{day.date}}</p>
        <p class="card-text">{{day.temparature}}</p>
      </div>
    </div>
    
   
  </div>
</div>
 </div>
    </template>
    
    <script>
import axios from 'axios'; 
import moment from 'moment' ; 

    export default {
      name: 'App',
      components: {

      },
      props: {
        cityname: String , 
      },
      data() {
        return {
          forecast: [],
          loading: true , 
        }
      },
      mounted() {
        this.fetchWeatherData()
      },
      methods: {
        async fetchWeatherData() {
          const apikey = "c2096da34a8169cfbdb5e1e2ba35bbc8";
          const city = this.cityname;
          const apiurl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apikey}`;
          await axios.get(apiurl).then(Response => {
              // console.log(Response,"wrking ") ; 4
              const forecastData = Response.data.list ; 
              const filteredData = forecastData.map( item=> {
                return {
                  date: moment(item.dt_txt.split(' ',[0])),
                  temparature: Math.round(item.main.temp),
                  description: item.weather[0].description 
                } ; 
              }).reduce((acc,item) => {
              if(!acc.some(day => day.date.isSame(item.date, 'day'))) {
                acc.push(item) ; 
              }
              return acc ; 
          }, []).slice(1.5);
          console.log(filteredData,"working") ;
             this.forecat = filteredData ; 
             this.loading = false  ;

          }).catch(error => {
            console.error('Error fetching weather data',error) ;
            this.loading = false ; 
          }) ; 

        }
      }
  
    }
    </script>
    
    <style>

    </style>