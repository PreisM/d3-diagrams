<template>
  <div id="app">
    <flower-chart />
    <pie-chart />
    <pie-chart2 />
    <chart-maker />
    <chartMakerStiff />
    <star />
    <bars />
    <burn-chart />
    <line-chart :class="[toggleClass]" />
    <button v-on:click="play">Play again</button>
  </div>
</template>

<script>
import chartMaker from './components/chartMaker.vue'
import chartMakerStiff from './components/chartMakerStiff.vue'
import star from './components/star.vue'
import bars from './components/bars.vue'
import burnChart from './components/burnChart.vue'
import lineChart from './components/lineChart.vue'
import pieChart from './components/pieChart.vue'
import pieChart2 from './components/pieChart2.vue'
import flowerChart from './components/flowerChart.vue'

export default {
  name: 'App',
  components: {
    chartMaker,
    chartMakerStiff,
    star,
    bars,
    burnChart,
    lineChart,
    pieChart,
    pieChart2,
    flowerChart
  },
  data() {
    return {
      toggleClass: "ani1"
    };
  },

  methods: {
    play() {
      // toggle classes to animate the line draw
      this.toggleClass == "ani1"
        ? (this.toggleClass = "ani2")
        : (this.toggleClass = "ani1");
    }
  }
}
</script>
<style>
body {
  background-color: rgba(0, 0, 0, 0.425);
}
button {
  display: inline-block;
  border: none;
  padding: 1rem 2rem;
  margin: 0;
  text-decoration: none;
  background: url(http://ui-design-coder.com/wp-content/themes/twentyfifteen/css/playBtn.ce805501.svg) no-repeat top left;
  background-size: contain;
  color: transparent;
  font-family: sans-serif;
  font-size: 1rem;
  cursor: pointer;
  text-align: center;
  transition: background 250ms ease-in-out, transform 150ms ease;
  -webkit-appearance: none;
  -moz-appearance: none;
  width: 8em;
}
button:focus {
  border: none;
}

button#playBtn {
  fill: red;
}

button:focus {
  outline: none;
}
</style>
