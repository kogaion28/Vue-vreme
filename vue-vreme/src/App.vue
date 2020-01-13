<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
            <main>
              <div class="search-box">
                <input type="text" class="search-bar" placeholder="Cauta..." v-model="query" @keypress="fetchWeather"/>
              </div>
        <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="data">{{ dateBuilder() }}</div>
          </div>
                <div class="weather-box">
                  <div class="temp">{{ Math.round(weather.main.temp) }} C</div>
                  <div class="weather">{{ weather.weather[0].main }}</div>
                </div>
        </div>

            </main>
  </div>
</template>

<script>


export default {
  name: 'app',
  data() {
    return{
      api_key:'f6a2c317f97367bfe8fa3e5fc517e80d',
      url_base:'https://api.openwathermap.org/data/2.5/',
      query: '',
      weather:{}
    }
  },
      methods:{
        fetchWeather (e){
          if (e.key == "Enter"){
            fetch('${this.url_base}weather?q=${this/query}&units=metric&APPID=${this.api_key}')
            .then(res => {
              return res.json();
            }).then(this.setResults);
          }
        },
        setResults (results){
          this.weather = results;
        },
        dateBuilder () {
          let d = new Date();
          let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
           let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

         let day = days[d.getDay()];
         let date = d.getDate();
         let month = months[d.getMonth()];
         let year = d.getFullYear();
         
         return '${day} ${date} ${month} ${year}'; 
        }
      }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat' , sans-serif;
}

#app {
  background-image: url('./assets/rece.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/cald.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px; 
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;

}

.search-box .search-bar:focus{
  box-shadow:  0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color:  rgba(255,255,255, 0.75);
  border-radius:  16px 0px 16px 0px;
}




</style>
