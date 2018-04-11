<template>
  <div id="app" class="container">
    <div id="header">
      <h1>D3 Money Vue</h1>
      <h2>Visualize Your Finances</h2>
    </div>
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
      Budgeting
      //Investing
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
  body{
    background-color: #cccccc;
  }
  h1, h3{
    font-weight: bold;
  }
  .row{
    background-color: whitesmoke;
    padding: 20px;
    /* /margin-top: 20px; */
  }
  th{
    font-size: 12px;
  }
</style>
