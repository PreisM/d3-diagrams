<template>
  <div id="app">
    <chart-maker />
    <chartMakerStiff />
    <star />
    <bars />
  </div>
</template>

<script>
import chartMaker from './components/chartMaker.vue'
import chartMakerStiff from './components/chartMakerStiff.vue'
import star from './components/star.vue'
import bars from './components/bars.vue'

export default {
  name: 'App',
  components: {
    chartMaker,
    chartMakerStiff,
    star,
    bars
  }
}
</script>

<style>
body {
  background-color: rgba(0, 0, 0, 0.425);
}
</style>
