<template>
  <div>
    
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  mounted () {
    this.dupa()
  },
  methods: {
    dupa () {
      const data = [35, 15, 15, 35]
      const div = d3.select('body')
      const width = window.innerWidth
      const height = window.innerHeight
      const radius = Math.min(width, height) / 2
      const colorScale = d3.scaleOrdinal(['#7326AB', '#2A59A9', '#E5A1D4', '#00a0b0'])

      const svg = div.append('svg')
        .attr('width', width)
        .attr('height', height)
        //przekształcenie grupy elementów
        .append('g')
        // wycentrowanie
          .attr('transform', `translate(${width / 2}, ${height / 2})`)

      const pie = d3.pie().value(d => d).sort(null)
      const arc = d3.arc()
      //wielkość
        .outerRadius(radius * 0.5)
      // donut
        .innerRadius(0)
      // wycięcie części okręgu
        // .startAngle(-0.5 * Math.PI)
        // .endAngle(0.5 * Math.PI)

      const g = svg.selectAll('.arc')
        .data(pie(data))
        .enter().append('g')
        .attr('class', 'arc')

      g.append('path')
        .attr('d', arc)
        .attr('class', 'arc')
        .style('fill', (d, i) => colorScale(i))
        .style('stroke', '#11141C')
        .style('stroke-width', 4)
    }
  }
}
</script>
<!-- css loaderhttps://vue-loader.vuejs.org/guide/scoped-css.html#mixing-local-and-global-styles -->
<style>

</style>
