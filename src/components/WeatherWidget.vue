<template>
  <div class="weather_widget">
    <v-card
      width="375"
      height="808"
      color=#222222>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="headline mb-1">Historical Weather</v-list-item-title>
          <v-list-item-title class="subtitle">Average Last 3 Days</v-list-item-title>
          <v-layout class="justify-center text-fields">
            <v-flex xs12 sm4>
            <v-text-field
              v-model="latitude"
              label="Latitude"
            ></v-text-field>
            </v-flex>
            <v-flex xs12 sm4>
              <v-text-field
                v-model="longitude"
                label="Longitude"
              ></v-text-field>
            </v-flex>
          </v-layout>
          <v-btn text color="white" @click="$emit('getData')">submit</v-btn>
        </v-list-item-content>
      </v-list-item>
      <v-flex class="d-flex">
        <v-list-item class="justify-center">
          <v-list-item-icon>
            <v-img
              src='../assets/temp.png'
              style="font-size=28px"
            >
            </v-img>
          </v-list-item-icon>
        </v-list-item>

        <v-list-item class="justify-center">
          <v-list-item-icon>
            <v-img
              src='../assets/precip.png'
            >
            </v-img>
          </v-list-item-icon>
        </v-list-item>
      </v-flex>
      <v-flex class="d-flex">
        <v-list-item-subtitle><h2>{{temp}}</h2></v-list-item-subtitle>
        <v-list-item-subtitle><h2>{{precip}}</h2></v-list-item-subtitle>
      </v-flex>

      <v-flex class="d-flex">
        <v-list-item-subtitle><h3>Temperature (F)</h3></v-list-item-subtitle>
        <v-list-item-subtitle><h3>Precipitation (in)</h3></v-list-item-subtitle>
      </v-flex>

      <v-spacer></v-spacer>

      <v-flex class="d-flex justify-space-around">
        <v-list-item class="justify-center">
          <v-list-item-icon>
            <v-img
              src='../assets/wind.png'
            >
            </v-img>
          </v-list-item-icon>
        </v-list-item>

        <v-list-item class="justify-center">
          <v-list-item-icon>
            <v-img
              src='../assets/wind-direction.png'
            >
            </v-img>
          </v-list-item-icon>
        </v-list-item>
      </v-flex>
      <v-flex class="d-flex">
        <v-list-item-subtitle><h2>{{wind}}</h2></v-list-item-subtitle>
        <v-list-item-subtitle><h2>{{windDir}}</h2></v-list-item-subtitle>
      </v-flex>

      <v-flex class="d-flex">
        <v-list-item-subtitle><h3>Wind (mph)</h3></v-list-item-subtitle>
        <v-list-item-subtitle><h3>Wind Direction</h3></v-list-item-subtitle>
      </v-flex>

      <v-spacer></v-spacer>

      <div class="divider"></div>

      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="headline mb-1">Forecast</v-list-item-title>
          <v-list-item-title class="subtitle">Something goes here</v-list-item-title>


          <v-card-text>
            <v-row class="justify-center" align="center">
              <v-col cols="4">
                <v-img
                  :src="currentConditions.weatherIcon"
                  alt="Sunny image"
                  width="92"
                ></v-img>
              </v-col>
              <v-col class="display-3 todays-forecast" cols="4">
                {{currentConditions.temp}}&deg;F
              </v-col>
            </v-row>
            <v-list-item class="justify-center">
              <h2>{{currentConditions.weatherDescription}}</h2>
            </v-list-item>
            <v-list-item-subtitle>
              <h3>Wind: {{currentConditions.windSpeed}} mph ({{currentConditions.windDir}})</h3>
            </v-list-item-subtitle>
          </v-card-text>

        </v-list-item-content>
      </v-list-item>

    </v-card>
  </div>
</template>

<script>
export default {
  name: "WeatherWidget",
  props: ["wind", "windDir", "temp", "precip", "currentConditions"],
  data: () => ({
    latitude: "",
    longitude: ""
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
  }
}
</script>

<style lang="scss">
  .headline{
    color: #FFFFFF;
    font-family: "Atlas Grotesk";
    font-size: 18px;
  }
  .subtitle{
    color: #FFFFFF;
  }
  .text-fields{
    color: #FFFFFF;
  }
  .spacer{
    height: 29px;
  }
  h2 {
    font-family: "Atlas Grotesk";
    font-size: 28px;
    line-height: 32px;
    text-align: center;
    color: #A5A5A5;
    margin-right: 34px;
  }
  h3 {
    font-family: "Atlas Grotesk";
    font-size: 12px;
    line-height: 18px;
    margin-right: 25px;
    text-align: center;
    color: #A5A5A5;
  }

  div .divider {
    width: 345px;
    height: 1px;
    background-color: #555555;
    margin: auto;
  }

  .v-application .display-3 {
    color: #A5A5A5;
  }
</style>