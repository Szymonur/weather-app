<template>
    <WeatherComponent> </WeatherComponent>
</template>

<script>
import WeatherComponent from './components/WeatherComponent.vue'

export default {
  name: 'App',
  components: {
    WeatherComponent
  }
}
</script>

<style>
body{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  background: rgb(15,148,214);
  background: linear-gradient(90deg, rgba(15,148,214,1) 0%, rgba(44,74,255,1) 35%, rgba(0,212,255,1) 100%);
  margin: 0;
  padding: 0;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
}
</style>
