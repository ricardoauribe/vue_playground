<template>
  <div id="app">
    <h1>Nutrition App</h1>
    <MyComponent/>
    <summary-graph/>
  </div>
</template>

<script>
import MyComponent from './components/MyComponent.vue'
import SummaryGraph from './components/SummaryGraph.vue'

export default {
  name: 'App',
  components: {
    MyComponent,
    SummaryGraph
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
