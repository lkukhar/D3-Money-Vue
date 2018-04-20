<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
      <div class="container">
        <h1 class="header">D3 Money Vue</h1>
        <small class="header">Visualize Your Finances</small>
      </div>
    </nav>
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
  h1, h3{
    font-weight: bold;
  }
  .header{
    color: white;
  }
  section{
    padding: 50px 0;
  }
  .row{
    padding: 20px 0;
  }

</style>
