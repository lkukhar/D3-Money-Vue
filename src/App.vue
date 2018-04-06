<template>
  <div id="app">
    <Finances/>
  </div>
</template>

<script>
import Investing from './components/Investing'

export default {
  name: 'App',
  components: {
    Investing
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
