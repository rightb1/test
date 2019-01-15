<template>
  <div id="app">
    <MyWeather/>
    </div>
</template>

<script>
import MyWeather from './components/MyWeather'

export default {
  name: 'App',
  components: {
    MyWeather
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
