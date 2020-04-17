<template>
  <div id="app" 
  :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''"
  >
    <main>
      <div class="search-box">
        <input type="text" 
        class="search-bar" 
        placeholder="Search city..."
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>
     
     <div class="weather-wrap"
      v-if="typeof weather.main != 'undefined'"
     >
      <div class="location-box">
       <div class="location"> {{ weather.name }}, {{ weather.sys.country }} </div>
       <div class="date">{{ dateBuilder() }}</div>
      </div>

      <div class="weather-box">
       <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
       <div class="weather">{{ weather.weather[0].main }}</div>
       </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data (){
    return{
      api_key: '72f29eb8ed5119321bc50c22f18e4cb9',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e){
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then( res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder (){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

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
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

#app{
  background: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.3s;
}

#app.warm{
  background: url(./assets/warm-bg.jpg);
  background-size: cover;
  background-position: bottom;
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.30), rgba(0,0,0,0.80));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 30px;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,.2), 0 6px 20px 0 rgba(0,0,0,.19);
  transition: 0.5s;
}

.search-box .search-bar:focus{
  box-shadow: 0 4px 8px 0 rgba(0,0,0,.4), 0 6px 20px 0 rgba(0,0,0,.25);
  background-color: rgba(255, 255, 255, 0.75);
  transition: 0.5s;
}

.weather-wrap .location{
  color: #fff;
  text-align: center;
  font-size: 35px;
  font-weight: 500;
  text-shadow: 2px 5px rgba(0,0,0,.25);
}

.location-box .date{
  color: #fff;
  text-align: center;
  font-size: 18px;
  font-weight: 300;
  font-style: italic;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  font-size: 6em;
  font-weight: 900;
  color: #fff;
  background: rgba(255, 255, 255, 0.35);
  text-shadow: 3px 6px rgba(0,0,0,.25);
  border-radius: 15px;
  margin: 20px 0;
  box-shadow: 2px 4px rgba(0,0,0,.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 35px;
  font-weight: 700;
  font-style: italic;
  text-shadow:2px 4px rgba(0,0,0,.25);
}

</style>
