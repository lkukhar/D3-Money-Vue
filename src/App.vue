<template>
  <div id="app" class="container">
    <h1 class="text-center">D3 Money Vue</h1>
    <h2 class="text-center">Visualize Your Finances</h2>
    <Budgeting />
  </div>
</template>

<!-- D3 CDN Link -->
<script src="https://d3js.org/d3.v5.min.js"></script>
<script>
  import Budgeting from './components/Budgeting'
  //import Investing from './components/Investing'

  export default {
    name: 'App',
    components: {
      //Investing
      Budgeting
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
