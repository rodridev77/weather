<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Pesquisar cidade..."
        v-model="query" @keypress="getWeather">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">
            {{weather.name}}, {{weather.sys.country}}
          </div>
          <div class="date">{{this.getDate()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}} °C</div>
          <div class="temp-details">
            <img class="weather-icon" v-bind:src="'https://openweathermap.org/img/w/'+weather.weather[0].icon+'.png'" >
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
      </div>
      <div class="weather-wrap" v-else>
        <div class="not-found">
          {{errorMessage}}
        </div>
      </div>
    </main>
  </div>
</template>

<script>

import api from '@/services/api.js';

export default {
  name: 'App',
  data() {
    return {
      api_key: 'your-api-key-here',
      query: '',
      weather: {},
      errorMessage: ''
    }
  },
  methods: {
    getWeather(event) {
      if (event.keyCode === 13) {
        api.get(`/weather?q=${this.query}&units=metric&lang=pt&appid=${this.api_key}`).then(response => {
          console.log(response.data.weather[0].main)
            if (response.data.name.length > 0) {
              this.weather = response.data;
              this.query = '';
              this.weather.weather[0].main = this.getWeatherMain(response.data.weather[0].main);
            } else {
              console.log('not found');
            }
        })
        .catch(err => {
           if (err.response.status) {
             //alert(err.response.status)
             this.weather.main = undefined;  
             this.errorMessage = 'Cidade não encontrada!';
             this.query = '';
           }
        });
      }
    },
    getDate() {
      let date = new Date();
      let months = [
        "Janeiros", "Fevereiro", "Março", "Abril", "Junho",
        "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"
      ];
      let days = ["Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta"];

      let day = days[date.getDay()];
      let today = date.getDate();
      let month = months[date.getMonth() - 1];
      let year = date.getFullYear();

      return `${day}, ${today} de ${month} de ${year}`;
    },
    getWeatherMain(main) {
      switch(main) {
        case 'Clear':
          return "Limpo";
        case 'Clouds':
          return "Núvens";
        case 'Rain':
          return "Chuva";
      }
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-size: sans-serif;
  color: #fff;
}

html, body {
  width: 100%;
  height: 100%;
}

body {
  font-family: sans-serif;
}

#app, template {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  display: flex !important;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,.75));
}

.search-box {
  width: 100%;
  max-width: 600px;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: rgba(0, 0, 0, .4);
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(255, 255, 255, .25);
  background-color: rgba(255, 255, 255, .5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 8px rgba(255, 255, 255, .25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location, .not-found {
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(255, 255, 255, .25);
}

.location-box .date {
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,.25);
}

.temp-details {
  display: flex;
  justify-content: center;
}

.weather-icon {
  display: inline-block;
  height: 70px;
  width: 70px;
}

.weather-box .weather {
  display: inline-block;
  font-size: 45px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,.25);
}
</style>
