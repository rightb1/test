<template>
  <div class="hello">
    <div class="search">
        <input v-model="newCity" placeholder="Enter city name">
        <button @click="addCity">Add City</button>
        <button class="btn btn-success" @click="search">Search</button></div>
    <div>
    <div>
        my cityes
    <div v-for="(city,n) in cities">
        <p>
            <span class="city" @click="seeWeather(n)">{{city}}</span> <button @click="removeCity(n)">Remove</button>
        </p>
    </div>
    </div>
        <div> weather in {{placeName}} {{country}}</div>
        <div> overcast {{overcast}}</div>
        <span class="temperature">currentTemp {{currentTemp}}°</span><br>
        <span id="temp-values">Min {{minTemp}}° <br> Max {{maxTemp}}°</span>
    </div>
    <div>
        <div> sunrise {{sunrise}}</div>
        <div> sunset {{sunset}}</div>
        <div> humidity {{humidity}}</div>
        <div> pressure {{pressure}}</div>
        <div> Wind {{wind}}</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
   data() {
    return {
    apiLink:'http://api.openweathermap.org/data/2.5/weather?units=metric',
    apiKey: '&APPID=16f86fb95d29de5b39a40af937274cbe',
    cities:[],
    newCity: null,
    placeName: '',
    country: '',
    currentTemp: '',
    minTemp: '',
    maxTemp:'',
    sunrise: '',
    sunset: '',
    pressure: '',
    humidity: '',
    wind: '',
    overcast: '',
    city:''
  }
},
  methods: {
    getWeather(url) {
      axios
        .get(url)
        .then(response => {
          this.placeName = response.data.name;
          this.country = response.data.sys.country;
          this.currentTemp = response.data.main.temp;
          this.minTemp = response.data.main.temp_min;
          this.maxTemp = response.data.main.temp_max;
          this.pressure = response.data.main.pressure;
          this.humidity = response.data.main.humidity + '%';
          this.wind = response.data.wind.speed + 'm/s';
          this.overcast = response.data.weather[0].description;
          this.icon = "images/" + response.data.weather[0].icon.slice(0, 2) + ".svg";
          this.sunrise = new Date(response.data.sys.sunrise*1000).toLocaleTimeString("en-GB").slice(0,5);
          this.sunset = new Date(response.data.sys.sunset*1000).toLocaleTimeString("en-GB").slice(0,5);
      })
      .catch(error => {
        console.log(error);
      });
    },
    search(){
        let url = this.apiLink + '&q=' + this.newCity + this.apiKey;
        this.getWeather(url);
    },
    seeWeather(x){
        console.log()
        let url = this.apiLink + '&q=' + this.cities[x] + this.apiKey;
        this.getWeather(url);
    },
    geolocation() {
      navigator.geolocation.getCurrentPosition(this.buildUrl, this.geoError);
    },
    buildUrl(position) {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;

      this.getWeather(this.apiLink + '&lat=' + lat + '&lon=' + lon + this.apiKey);
    },
    geoError(error) {
      this.getWeather(this.apiLink + '&lat=0&lon=0' + this.apiKey);
    },
    addCity() {
      // ensure they actually typed something
      if(!this.newCity) return;
      this.cities.push(this.newCity);
      this.newCity = '';
      this.savecities();
    },
    removeCity(x) {
      this.cities.splice(x,1);
      this.savecities();
    },
    savecities() {
      let parsed = JSON.stringify(this.cities);
      localStorage.setItem('cities', parsed);
    }
  },
  beforeMount() {
    this.geolocation();
  },
  mounted(){
     if(localStorage.getItem('cities')) {
      try {
        this.cities = JSON.parse(localStorage.getItem('cities'));
      } catch(e) {
        localStorage.removeItem('cities');
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.hello{
    border: 1px solid grey;
}
</style>
