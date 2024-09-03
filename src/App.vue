<template>
  <div id="app">
    <SpaceScene />
  </div>
</template>

<script>
import SpaceScene from './components/SpaceScene.vue'

export default {
  name: 'App',
  components: {
    SpaceScene
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
