<template>
        <div class="container p-0">
            <div class="d-flex">
                <div class="card main-div w-100">
                <div class="p-3">
                    <h2 class="mb-1 day">Tuesday</h2>
                    <small>{{time}}</small>
                    <h2 class="place"><i class="fa fa-location">{{name}} <small>{{country}}</small></i></h2>
                    <div class="temp">
                        <h1 class="">{{ temparature}}&deg; C</h1>
                        <h2 class="">{{description}} <img :src="iconUrl" ></h2>
                    </div>
                </div>
                </div>
            </div>
        </div>
        <div class="card card-2 w-100">
            <table class="m-4">
                <tbody>
                    <tr>
                    <th>Sea Level</th>
                    <th>{{sea_level}}</th>
                </tr>
                <tr>
                    <th>wind</th>
                    <th>{{wind}}</th>
                </tr>
                <tr>
                    <th>Country</th>
                    <th>{{country}}</th>
                </tr>
                <tr>
                    <th>Humidity</th>
                    <th>{{ humidity }}</th>
                </tr>
                </tbody>
            </table>

            <DaysWeather :cityname="cityname" />
            <div id="div_Form" class="d-flex m-3 justify-content-center">
                <form action="">
                    <input type="button" value="change location" @click="changeLocation" class="btn change-btn btn-primary">

                </form>
            </div>
        </div>
</template>
    
    <script>
    import axios from 'axios'
    import DaysWeather from './DaysWeather.vue'
    export default {
      name: 'myWeather',
      components: { 
        DaysWeather,
      },
      props: {
        city: String,
      },
      methods: {
        changeLocation() {
            window.location.reload() ;
        } 
      },
      data() {
        return {
            cityname: this.city ,
            temparature: null,
            description: null,
            iconUrl: null,
            date: null,
            time: null,
            name: null ,
            sea_level:null ,
            wind: null ,
            country:null ,
            humidity: null ,
        }
      },
      async created() {
            const response =await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=c2096da34a8169cfbdb5e1e2ba35bbc8
`);
            const weatherData = response.data ; 
            this.temparature = weatherData.main.temp ; 
            this.description = weatherData.weather[0].description ; 
            this.name = weatherData.name; 
            this.wind = weatherData.wind.speed;
            this.sea_level = weatherData.main.sea_level;
            this.country = weatherData.sys.country ; 
            this.humidity = weatherData.main.humidity ; 
            this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png` ;
            const d = new Date();
            this.time = d.getHours() + ':'+d.getMinutes() + ':' + d.getSeconds() ;  

        }
      

    }
    </script>
    
    <style>
body {
    background-color: #343d4b;
}

.weather-temp {
margin: 0;
font-weight: 700;
font-size: 4px;
}
h2.mb-1.day {
    font-size: 3rem;
    font-weight: 400;
}

.main-div {
border-radius: 20px;
 color: #fff;
 background-image: url('https://unsplash.com/photos/a-view-of-a-valley-with-mountains-in-the-background-Oku99GcuUoY');
 background-position: center;
 background-blend-mode: overlay;
 background-color: rgba(red, green, blue, alpha);
 background-repeat: no-repeat;

}
    
    </style>