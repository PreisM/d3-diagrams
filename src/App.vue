<template>
  <div id="app">
    <chart-maker />
    <chartMakerStiff />
    <star />
  </div>
</template>

<script>
import chartMaker from './components/chartMaker.vue'
import chartMakerStiff from './components/chartMakerStiff.vue'
import star from './components/star.vue'

export default {
  name: 'App',
  components: {
    chartMaker,
    chartMakerStiff,
    star
  }
}
</script>

<style>
body {
  background-color: rgba(0, 0, 0, 0.425);
}
</style>
