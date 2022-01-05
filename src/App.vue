<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 22 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrapp" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>

      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      api_key: '35f968a02048d9ace7ac697452a81677',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {}
    }
  }, 
  methods:{
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()]
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
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
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: cover;
  transition: 0.4s ease-out;
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.2), rgba(0,0,0,0.75));
  padding: 25px
}

.search-box{
  width: 100%;
  margin: 0 0 75px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;

  appearance: none;
  border: none;
  outline: none;

  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 0px 16px 16px;
  margin-top: -25px;
  box-shadow:  0px 5px rgba(0, 0, 0, 0.2);

  color: #313131;
  font-size: 20px;

  transition: 0.4s ease
}

.search-box .search-bar:focus{
  background-color: rgba(255, 255, 255, 0.75)
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}


.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 2px 2px rgba(50, 50, 70, 0.5);
}

.weather-box  {
  text-align: center;
}

.weather-box .temp{
  position: relative;
  display: inline-block;
  margin: 30px auto;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 16px;

  padding: 15px 25px;

  color: white;
  font-size: 102px;
  font-weight: 400;
  text-shadow: 3px 6px rgba(50, 50, 70, 0.5);
  text-align: center;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.2);
}

.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}
</style>
