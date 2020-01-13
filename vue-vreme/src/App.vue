<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'cald' : ''">

    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="bara-cautar" 
          placeholder="Cauta Locatia"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
       <!-- <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp >  ? 'ploios' : ''">

    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="bara-cautar" 
          placeholder="Cauta Locatia"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div> -->

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="cutie-vreme">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="vreme-cutie">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: 'f6a2c317f97367bfe8fa3e5fc517e80d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["Ianuarie", "Februarie", "Martie", "Aprilie", "Mai", "Iune", "Iulie", "August", "Septembrie", "Octobrie", "Noiembrie", "Decembrie"];
      let days = ["Luni", "Marti", "Miercuri", "Joi", "Vineri", "Sambata", "Duminica"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
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
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/vreme-rece.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.cald {
  background-image: url('./assets/cald.jpg');
}

#app.ploios{
  background-image: url('./assets/ploaie.jpg')
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .bara-cautar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .bara-cautar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.cutie-vreme .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.cutie-vreme .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.vreme-cutie {
  text-align: center;
}

.vreme-cutie .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.vreme-cutie .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
