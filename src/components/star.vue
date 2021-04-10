<template>
  <div class="justify-center items-center text-center flex">
    <svg width="500" height="500">
      <defs>
        <radialGradient id="starGradient">
          <stop offset="2%" stop-color="white" />
          <stop offset="95%" :stop-color="starColor" />
        </radialGradient>
      </defs>
      <rect width="100%" height="100%" fill="black" />
      <path
        class="radial"
        :d="radialData"
        transform="translate(250, 250)"
        fill="url(#starGradient)"
      ></path>
    </svg>
    <div class="my-auto">
      <div>
        <input
          type="range"
          name="rays"
          min="4"
          max="60"
          v-model="rays"
        />
        <label for="rays">Rays</label>
      </div>
      <div>
        <input
          type="range"
          name="radius"
          min="10"
          max="1000"
          v-model="radius"
        />
        <label for="radius">Radius</label>
      </div>
      <div>
        <input
          type="range"
          name="heat"
          min="1"
          max="100"
          v-model="heat"
        />
        <label for="rays">Heat</label>
      </div>
    </div>
  </div>
</template>

<script>
import { scaleLinear } from 'd3-scale';
import { lineRadial } from 'd3-shape';
export default {
  data() {
    return {
      radius: 300,
      rays: 8,
      heat: 5,
    };
  },
  computed: {
    outerRadius() {
      const scale = scaleLinear()
        .domain([10, 1000])
        .range([5, 250]);
      return scale(this.radius)
    },
    innerRadius() {
      return this.outerRadius * 0.5
    },
    radialPoints() {
      const step = (2 * Math.PI) / (this.rays * 2)
      const points = []
      for (let i = 0; i <= this.rays * 2; i++) {
        const currentRadius = i % 2 ? this.innerRadius : this.outerRadius
        points.push([i * step, currentRadius])
      }
      return points
    },
    radialData() {
      const radialLineGenerator = lineRadial()
      return radialLineGenerator(this.radialPoints)
    },
    starColor() {
      const myColor = scaleLinear()
        .domain([0, 25, 50, 75, 100])
        .range(['#ff7665', '#ffb469', '#ffe876', '#fff', '#99cdff'])
      return myColor(this.heat)
    },
  },
};
</script>