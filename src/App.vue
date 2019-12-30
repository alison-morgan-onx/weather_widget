<template>
  <v-app>
    <v-content>
      <WeatherWidget
      @updateLatitude="latitude = $event"
      @updateLongitude="longitude = $event"
      @getData="requestWeatherData()"
      :currentConditions="currentConditions"
      :wind="avgWind"
      :windDir="avgWindDirDeg"
      :temp="avgTemp"
      :precip="avgPrecip"
      />
    </v-content>
  </v-app>
</template>

<script>
import WeatherWidget from './components/WeatherWidget'

export default {
  name: 'App',

  components: {
    WeatherWidget
  },

  data: () => ({
    averagesToFind: ["windspeedMiles", "precipInches", "winddirDegree"],
    avgPrecip: 0,
    avgTemp: 0,
    avgWind: 0,
    avgWindDirDeg: "",
    currentConditions: {
      temp: 0,
      weatherDescription: "",
      weatherIcon: "",
      windDir: 0,
      windSpeed: 0,
    },
    endDate: "",
    latitude: 46.8721,
    longitude: -113.9940,
    startDate: "",
    weatherData: [],
  }),
  watch: {
    latitude(){
      this.$emit('updateLatitude', this.latitude)
      return this.latitude
        
    },
    longitude(){
      this.$emit('updateLongitude', this.longitude)
      return this.longitude
    }
  },
  methods:{
    formatDate(date){
      let month = '' + (date.getMonth() + 1)
      let day = '' + date.getDate()
      let year = date.getFullYear()

      if (month.length < 2) 
          month = '0' + month;
      if (day.length < 2) 
          day = '0' + day;

      return [year, month, day].join('-');
    },
    averageTemp(){
      let temp = 0;
      for (let day in this.weatherData) {
        temp = temp + Number(this.weatherData[day]["avgtempF"])
      }
      this.avgTemp = Math.round(temp / 3)
    },
    averageValues(avg){
      let avg1 = 0;
      let avg2 = 0;
      for (let day in this.weatherData) {
        for (let hour in this.weatherData[day].hourly) {
          avg1 = avg1 + Number(this.weatherData[day].hourly[hour][avg])
        }
        avg2 = avg1 / 8
      }
      if (avg === "windspeedMiles") this.avgWind = Math.round(avg2 / 3)
      else if (avg === "precipInches") this.avgPrecip = Math.round(avg2 / 3)
      else {
        this.avgWindDirDeg = this.convertDegToDirection(Math.round(avg2 / 3))
      }
    },
    convertDegToDirection(deg){
      const val = Math.floor((deg / 22.5) + 0.5);
      const arr = ["N", "NNE", "NE", "ENE", "E", "ESE", "SE", "SSE", "S", "SSW", "SW", "WSW", "W", "WNW", "NW", "NNW"];
      return arr[(val % 16)];
    },
    getAverages(){
      // Average temp for day
      this.averageTemp()

      // Averages for wind, precip, wind direction these are calulated differently than temp
      for (let avg in this.averagesToFind) {
        this.averageValues(this.averagesToFind[avg])
      }

      // Populate data for todays conditions
      this.getCurrentConditions()
    },
    getCurrentConditions(){
      let date = new Date();
      let now = date.getHours();
      now = parseInt(now.toString() + "00")
      let currentHour = {};
      for (let hour in this.weatherData[2].hourly) {
        if (parseInt(this.weatherData[2].hourly[hour].time) <= now) {
          currentHour = this.weatherData[2].hourly[hour]
        } else {
          break
        }
      }
      this.currentConditions.temp = currentHour.tempF
      this.currentConditions.weatherDescription = currentHour.weatherDesc[0].value
      this.currentConditions.weatherIcon = currentHour.weatherIconUrl[0].value
      this.currentConditions.windDir = currentHour.winddir16Point
      this.currentConditions.windSpeed = currentHour.windspeedMiles
      console.log('current conditiions', this.currentConditions)
    },
    requestWeatherData(){
      fetch(`http://api.worldweatheronline.com/premium/v1/past-weather.ashx?key=7ad4cf6758ee4e4dae7180735192912&q=${this.latitude},${this.longitude}&format=json&date=${this.startDate}&enddate=${this.endDate}`, {
          method: "GET",
          headers : { 
              'Accept': 'application/json'
          }
        })
        .then(
          response => {
            response.json()
              .then((json) => {
                console.log(json)
                this.weatherData = json.data.weather
                this.getAverages()
              })
        })
        .catch(
          err => {
            console.error('could not return weather data', err)
          }
        )
    }
  },
  created() {
    let date = new Date();
    this.endDate = this.formatDate(date)

    // Get date string for 2 days ago
    date.setDate(date.getDate() - 2);
    this.startDate = this.formatDate(date)

  }
};
</script>
