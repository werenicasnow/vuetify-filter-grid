<template>
  <div id="app" data-app>
    <CustomGrid/>
  </div>
</template>

<script>
import CustomGrid from './components/CustomGrid'

export default {
  name: 'App',
  components: {
    CustomGrid
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
