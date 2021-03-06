<template>
  <div id="app">
    <chart-maker />
    <circleGraph />
  </div>
</template>

<script>
import chartMaker from './components/chartMaker.vue'
import circleGraph from './components/circleGraph.vue'

export default {
  name: 'App',
  components: {
    chartMaker,
    circleGraph
  }
}
</script>

<style>
body {
  background-color: rgba(0, 0, 0, 0.425);
}
</style>
