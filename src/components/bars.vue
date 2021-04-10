<template>
<div>
  <svg id="chart" :width="chartWidth" :height="chartHeight">
    <g
      v-for="(value, i) in animatedChartData"
      :key="value"
      :transform="`translate(0, ${i * 40 + 15})`"
    >
    <rect height="30" :width="barWidth(value)" fill="#f1f1f1" />
      <text y="20" :x="barWidth(value) - 25">
        {{ round(value) }}
      </text>
    </g>
  </svg>
  <button class="border border-gray-100 p-2 m-2" @click="changeData">Change</button>
  </div>
</template>

<script>

export default ({
  data () {
    return {
      dataMax: 20,
      chartWidth: 500,
      chartHeight: 300,
      chartData: [5, 8, 20, 3, 17, 14],
      animatedChartData: [0, 0, 0, 0, 0, 0]
    }
  },
  mounted () {
    this.animate();
  },
  watch: {
    chartData () {
      this.animate()
    }
  },
  methods: {
    barWidth (value) {
      return this.chartWidth / this.dataMax * value
    },
    changeData () {
      const randomData = new Array(6).fill('').map(() => 1 + Math.max(Math.random() * 20))
      this.chartData = randomData
    },
    animate () {
      const fromValues = this.animatedChartData
      const toValues = this.chartData
      const duration = 600
      const start = Date.now()

      const frame = () => {
        const elapsed = Date.now() - start
        const percent = elapsed / duration

        if (percent >= 1) {
          this.animatedChartData = this.chartData
        } else {
          this.animatedChartData = toValues.map((toVal, i) => {
            const fromVal = fromValues[i]
            return fromVal + (toVal - fromVal) * percent
          })
          requestAnimationFrame(frame)
        }
      }
    requestAnimationFrame(frame)
    },
    round(value) {
      return Math.round(value)
    }
  }
})
</script>
