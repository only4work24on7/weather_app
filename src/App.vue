<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' : 'app_w'">
    <main>
      <div class="search-box">
        <input 
        class="search-bar" 
        type="text"
        v-model="query" 
        placeholder="Search...then  press enter" 
        @keypress="fetchWeather" >
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' " >
        <div class="location-box">
          <div class="location"> {{weather.name}} , {{ weather.sys.country }}</div>
          
          <div class="date"> {{date_function()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round((weather.main.temp + Number.EPSILON) * 10) / 10}}<span>&#176;</span>c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
        
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: 'e0413771852a5c64989b207ba697b99d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
    }
  },
  methods: {
    fetchWeather(e){
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results){
      this.weather = results;
    },
     date_function() {
      const currentDate = new Date();
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      const days   = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let month    = months[currentDate.getMonth()];
      let dayW     = days[currentDate.getDay()];
      let dayM     = currentDate.getDate();
      let year     = currentDate.getFullYear();
      return `${dayW}, ${dayM}, ${month}, ${year}`; 


     
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
  font-family: sans-serif;
}
.app_w, .warm {
  height: 100vh;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.app_w {
  background-image: url('./images/cold.jpg');
}
.warm {
  background-image: url('./images/hot.jpg');
}



main {
  background-image: linear-gradient( to bottom,rgba( 0,0,0, 0.25),rgba( 0,0,0, 0.5));
  min-height: 100vh;
  padding: 25px;
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  width: 100%;
  padding: 15px;
  color: #313131;
  appearance: none;
  border: none;
  outline: none;
  background-color: none;
  background-color: rgba( 255,255,255, 0.5);
  transition: 0.4s;
  border-radius: 16px 0;
  box-shadow: 0 0 8px rgba( 0,0,0, 0.5);
}
.search-bar:focus {
  background-color: rgba( 255,255,255, 0.75);
  border-radius: 0 16px;
  box-shadow: 0 0 16px rgba( 0,0,0, 0.5);

}
.location {
  text-align: center;
  color: #fff;
  font-size: 38px;
  font-weight: 900;
  text-shadow: 0 0 15px rgba( 0,0,0, 0.5);
}
.date {
  text-align: center;
  color: #fff;
  font-style: italic;
  font-size: 20px;
}
.weather-box{
  text-align: center;
}
.temp {
  display: inline-block;
  text-align: center;
  color: #fff;
  font-size: 90px;
  background-color: rgba( 255,255,255, 0.35);
  padding: 15px 30px;
  margin: 10px 0;
  box-shadow: 0 0 16px rgba( 0,0,0, 0.25);
  border-radius: 15px;
  font-weight: 900;

}
.weather-box .weather {
  text-align: center;
  color: #fff;
  font-style: italic;
  font-size: 50px;
  font-weight: 800;
  text-shadow: 0 0 12px rgba( 0,0,0, 0.5);
}

</style>
