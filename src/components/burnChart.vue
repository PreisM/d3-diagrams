<template>
  <svg id="burnChart" width="1200" height="750" style="background-image: url('https://etc.usf.edu/presentations/backgrounds/grid/grid_04/1304m.gif');">
  </svg>

</template>

<script>
import * as d3 from "d3";
import { nest } from 'd3-collection';

export default {
  data () {
    return {
      data: [
        { year: 2016, media: "Digital", spending: 72.2 },
        { year: 2017, media: "Digital", spending: 90.39 },
        { year: 2018, media: "Digital", spending: 107.3 },
        { year: 2019, media: "Digital", spending: 125.75 },
        { year: 2020, media: "Digital", spending: 142.23 },
        { year: 2021, media: "Digital", spending: 156.43 },
        { year: 2022, media: "Digital", spending: 170.48 },
        { year: 2016, media: "TV", spending: 71.29 },
        { year: 2017, media: "TV", spending: 70.22 },
        { year: 2018, media: "TV", spending: 69.87 },
        { year: 2019, media: "TV", spending: 69.17 },
        { year: 2020, media: "TV", spending: 69.52 },
        { year: 2021, media: "TV", spending: 68.82 },
        { year: 2022, media: "TV", spending: 68.13 },
        { year: 2016, media: "Print", spending: 25.49 },
        { year: 2017, media: "Print", spending: 22.81 },
        { year: 2018, media: "Print", spending: 20.05 },
        { year: 2019, media: "Print", spending: 17.29 },
        { year: 2020, media: "Print", spending: 15.19 },
        { year: 2021, media: "Print", spending: 13.56 },
        { year: 2022, media: "Print", spending: 12.38 },
        { year: 2016, media: "Radio", spending: 14.33 },
        { year: 2017, media: "Radio", spending: 14.33 },
        { year: 2018, media: "Radio", spending: 14.41 },
        { year: 2019, media: "Radio", spending: 14.43 },
        { year: 2020, media: "Radio", spending: 14.46 },
        { year: 2021, media: "Radio", spending: 14.49 },
        { year: 2022, media: "Radio", spending: 14.52 },
        { year: 2016, media: "Out-of-home", spending: 7.6 },
        { year: 2017, media: "Out-of-home", spending: 7.75 },
        { year: 2018, media: "Out-of-home", spending: 7.87 },
        { year: 2019, media: "Out-of-home", spending: 7.95 },
        { year: 2020, media: "Out-of-home", spending: 8.03 },
        { year: 2021, media: "Out-of-home", spending: 8.11 },
        { year: 2022, media: "Out-of-home", spending: 8.19 },
        { year: 2016, media: "Directories", spending: 2.35 },
        { year: 2017, media: "Directories", spending: 1.83 },
        { year: 2018, media: "Directories", spending: 1.47 },
        { year: 2019, media: "Directories", spending: 1.19 },
        { year: 2020, media: "Directories", spending: 0.99 },
        { year: 2021, media: "Directories", spending: 0.84 },
        { year: 2022, media: "Directories", spending: 0.74 }
      ],
      leftMargin: 70,
      topMargin: 30,
    }
  },
  mounted () {
    this.burnChart()
  },
  methods: {
    burnChart () {
      //format daty
      const parseTime = d3.timeParse('%Y')
      this.data.forEach(function (d) {
        d.year = parseTime(d.year)
      })
      //dane
      const xExtent = d3.extent(this.data, d => d.year)
      const xScale = d3.scaleTime().domain(xExtent).range([this.leftMargin, 800])
      const yMax = d3.max(this.data, d=>d.spending)
      const yScale = d3.scaleLinear().domain([0, yMax+this.topMargin]).range([600,0])
      const xAxis = d3.axisBottom()
      .scale(xScale)

      //text pod wykresem
      d3.select('svg#burnChart')
      .append('g')
      .attr('class', 'axis')
      .attr('transform', 'translate(0, 620)')
      .call(xAxis)
      .append('text')
      .attr('x', (900+this.leftMargin)/2)
      .attr('y', '50')
      .text('Year')

      //oś lewa
      const yAxis = d3.axisLeft()
      .scale(yScale)
      .ticks(10)

      //text po lewo
      d3.select('svg#burnChart')
      .append("g")
      .attr("class", "axis")
      .attr("transform", `translate(${this.leftMargin},20)`)
      .call(yAxis)
      .append("text")
      .attr("transform", "rotate(-90)")
      .attr("x", "-150")
      .attr("y", "-50")
      .text("US Media Ad Spending (Billions)")

      const sumstat = nest()
      .key(d => d.media)
      .entries(this.data)

      console.log(sumstat)
      const mediaName = sumstat.map(d => d.key)
      const color = d3.scaleOrdinal().domain(mediaName).range(['#ff7665', '#ffb469', '#ffe876', '#000', '#99cdff'])

      // line
      d3.select("svg#burnChart")
      .selectAll(".line")
      .append("g")
      .attr("class", "line")
      .data(sumstat)
      .enter()
      .append("path")
      .attr("d", function (d) {
          return d3.line()
              .x(d => xScale(d.year))
              .y(d => yScale(d.spending)).curve(d3.curveCardinal)(d.values)     
      })
      .attr("fill", "none")
      .attr("stroke", d => color(d.key))
      .attr("stroke-width", 2)


      //circle 
      d3.select("svg#burnChart")
        .selectAll("circle")
        .append("g")
        .data(this.data)
        .enter()
        .append("circle")
        .attr("r", 6)
        .attr("cx", d => xScale(d.year))
        .attr("cy", d => yScale(d.spending))
        .style("fill", d => color(d.media))

      //legend
      var legend = d3.select("svg#burnChart")
        .selectAll('g.legend')
        .data(sumstat)
        .enter()
        .append("g")
        .attr("class", "legend");

      legend.append("circle")
          .attr("cx", 1000)
          .attr('cy', (d, i) => i * 30 + 350)
          .attr("r", 6)
          .style("fill", d => color(d.key))

      legend.append("text")
          .attr("x", 1020)
          .attr("y", (d, i) => i * 30 + 355)
          .text(d => d.key)

      //append title
      d3.select("svg#burnChart")
          .append("text")
          .attr("x", 485)
          .attr("y", 30)
          .attr("text-anchor", "middle")
          .text("US Total Media Ad Spending, by Media, 2016-2022")
          .style("fill", "black")
          .style("font-size", 28)
          .style("font-family", "Arial Black")

      //apend source
      d3.select("svg#burnChart")
          .append("text")
          .attr("x", 70)
          .attr("y", 700)
          .text("Source: eMarketer, March 2018")
          .style("fill", "black")
          .style("font-size", 14)
          .style("font-family", "Arial Black")
    }
  }
}
</script>

<style>
.axis path{
    stroke:black;
    stroke-width:2px ;
}   

.axis line{
    stroke: black;
    stroke-width: 1.5px;
} 

.axis text{
    fill: black;
    font-weight: bold;
    font-size: 14px;
    font-family:"Arial Black", Gadget, sans-serif;
} 

.legend text{
    fill:  black;
    font-family:"Arial Black", Gadget, sans-serif;
}
</style>