<template>
  <div id="app">
    <chart-maker />
  </div>
</template>

<script>
import chartMaker from './components/chartMaker.vue'

export default {
  name: 'App',
  components: {
    chartMaker
  }
}
</script>

<style>
body {
  background-color: rgba(0, 0, 0, 0.425);
}
</style>
