<template>
  <svg :width="width" :height="height">
    <g style="transform: translate(0, 10px)">
      <path :d="line"></path>
    </g>
  </svg>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'Chart',

  props: {
    width: {
      type: Number,
      required: false,
      default: 500
    },
    height: {
      type: Number,
      required: false,
      default: 200
    },
    data: {
      type: Array,
      required: true
    }
  },

  computed: {
    line() {
      const scale = this.getScales();
      const path = d3
        .line()
        .x((d, i) => scale.x(i))
        .y(d => scale.y(d));
      return path(this.data);
    }
  },

  methods: {
    getScales() {
      const x = d3.scaleTime().range([0, 430]);
      const y = d3.scaleLinear().range([210, 0]);
      d3.axisLeft().scale(x);
      d3.axisBottom().scale(y);
      x.domain(d3.extent(this.data, (d, i) => i));
      y.domain([0, d3.max(this.data, d => d)]);
      return { x, y };
    }
  }
};
</script>

<style scoped>
svg {
  margin: 25px;
}
path {
  fill: none;
  stroke: #abc;
  stroke-width: 2px;
}
</style>
