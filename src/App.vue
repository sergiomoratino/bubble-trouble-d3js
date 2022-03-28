<template>
  <div id="app">
    <PackChart />
  </div>
</template>

<script>
import PackChart from "./components/Chart.vue";



export default {
  name: 'App',
  components: {
    PackChart
  },
  mounted() {
    console.log("App loaded");
  },
}


/*var nodes = null;
var edges = null;*/




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
