<template>
  <div class="pie2">
    
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
        const data = [
          {
            type: 'Auto',
            cost_percent: 14.5
          },
          {
            type: 'Food',
            cost_percent: 28
          },
          {
            type: 'StudentLoan',
            cost_percent: 17
          },
          {
            type: 'Rent',
            cost_percent: 32.5
          },
          {
            type: 'Insurance',
            cost_percent: 8
          }
        ]
        const div = d3.select('.pie2')
        const width = window.innerWidth
        const height = window.innerHeight
        const radius = Math.min(width, height) / 2
        const colorScale = d3.scaleOrdinal(['#7326AB', '#2A59A9', '#E5A1D4', '#00a0b0', '#1C9FE9'])

        const svg = div.append('svg')
          .attr('width', width)
          .attr('height', height)
          .append('g')
            .attr('transform', `translate(${width / 2}, ${height / 2})`)

        const pie = d3.pie().value(d => d.cost_percent).sort(null)
        const arc = d3.arc().innerRadius(0).outerRadius(radius * 0.55)
        const arcText = d3.arc().innerRadius(0).outerRadius(radius * 0.75)
        //hover
        const hoverArc = d3.arc().innerRadius(0).outerRadius(radius * 0.65)

        const g = svg.selectAll('.arc')
          .data(pie(data))
          .enter().append('g')
          .attr('class', 'arc')

        g.append('path')
          .attr('d', arc)
          .attr('class', 'arc')
          .style('fill', (d, i) => colorScale(i))
          .style('fill-opacity', 0.8)
          .style('stroke', 'black')
          .style('stroke-width', 6)
          .on('mouseover', function () {
            d3.select(this)
              .style('fill-opacity', 1)
              .transition().duration(500)
              .attr('d', hoverArc)
          })
          .on('mouseout', function () {
            d3.select(this)
              .style('fill-opacity', 0.8)
              .transition().duration(500)
              .attr('d', arc)
          })

          //labels

          g.append('text')
            .attr('transform', d => `translate(${arcText.centroid(d)})`)
            .text(d => `${d.data.type} ${d.data.cost_percent}%`)
            .style('font-size', 16)
            .style('font-weight', 800)
            .style('fill', '#FFF')
            .style('text-anchor', 'middle')
            .style('text-shadow', '2px 2px #0E0B16')
    }
  }
}
</script>
<!-- css loaderhttps://vue-loader.vuejs.org/guide/scoped-css.html#mixing-local-and-global-styles -->
<style>

</style>
