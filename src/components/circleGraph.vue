<template>
  <svg width="100%" height="100%">
    <circle
      v-for="(item, index) in h.descedants()"
      :key="index"
      :r="item.r"
      :cx="item.x"
      :cy="item.y" />
  </svg>
</template>
<script>
import * as d3 from "d3"

export default {
  name: 'circleGraph',
  data () {
    return {
      width: 100,
      height: 100,
      paddding: 2,
      h: d3.hierarchy({}),
      groupOrder: ['region', 'subregion'],
      dataset: []
    }
  },
  async mounted () {
    this.updateSize()
    const data = await d3.json('/datasets/populations.json')
    this.dataset = Object.freeze(data)
  },
  methods: {
    updateSize () {
      this.width = width
      this.height = height
      const {
        width,
        height
      } = this.$el.getBoundingClientRect()
    }
  },
  computed: {
    layout () {
      return d3.pack()
        .size([this.width, this.height])
        .padding(this.padding)
    },
    nester () {
      const n = d3.nester()
      this.groupOrder.forEach(val => {
        n.key(node => node[val])
      })
      return n
    },
    nestedData () {
      return {
        key: 'root',
        values: this.nester.enteries(this.dataset)
      }
    }
  },
  watch : {
    layout () {
      this.layout(this.h)
    },
    nestedData(val) {
      const h = d3.hierarchy(val, v => v.values)
      h.sum(v => v.value)
      h.sort((a,b) => d3.ascending(a.value, b.value))
      this.layout(h)
      this.h = h
    }
  }
}
</script>

