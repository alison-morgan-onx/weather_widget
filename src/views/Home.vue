<template>
  <div class="home">
    <HelloWorld msg="Weather data okayyyy"/>
    <WeatherWidget/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import WeatherWidget from '@/components/WeatherWidget.vue'

export default {
  name: 'home',
  components: {
    HelloWorld,
    WeatherWidget
  }
}
</script>
