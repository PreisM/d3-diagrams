<template>
  <div class="border w-3/4 h-64 mx-auto my-12">
    <svg width="100%" height="100%" @click="addPoint" @contextmenu.prevent="points = []">
      <path fill="transparent" stroke="#fff" stoke-width="1" :d="lineGenerator(points)" />
      <circle v-for="(item, index) in points"
        :key="index"
        :r="5"
        :cx="item.x"
        :cy="item.y" />
    </svg>
  </div>
</template>
<script>
import * as d3 from "d3";

export default {
  name: 'chartMaker',
  data () {
    return {
      points: [
        {
          x: 200,
          y: 500
        }
      ]
    }
  },
  methods: {
    addPoint(ev) {
      const {
        layerX: x,
        layerY: y
      } = ev

      this.points.push({
        x,
        y
      })
    }
  },
    computed: {
      lineGenerator() {
        return d3.line()
        .x(item => item.x)
        .y(item => item.y)
        .curve(d3.curveNatural)
      }
    }
  }
</script>

