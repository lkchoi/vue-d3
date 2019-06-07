<template>
  <div class="chart__container" />
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'ForceDirectedGraph',

  props: {
    width: {
      type: Number,
      required: false,
      default: 600
    },
    height: {
      type: Number,
      required: false,
      default: 600
    },
    data: {
      type: Object,
      required: true
    }
  },

  methods: {
    chart(data) {
      const { width, height } = this;

      const svg = d3.create('svg').attr('viewBox', [0, 0, width, height]);
      const links = data.links.map(d => Object.create(d));
      const nodes = data.nodes.map(d => Object.create(d));
      const scale = d3.scaleOrdinal(d3.schemeCategory10);
      const color = d => scale(d.group);
      const drag = simulation => {
        function dragstarted(d) {
          if (!d3.event.active) simulation.alphaTarget(0.3).restart();
          d.fx = d.x;
          d.fy = d.y;
        }

        function dragged(d) {
          d.fx = d3.event.x;
          d.fy = d3.event.y;
        }

        function dragended(d) {
          if (!d3.event.active) simulation.alphaTarget(0);
          d.fx = null;
          d.fy = null;
        }

        return d3
          .drag()
          .on('start', dragstarted)
          .on('drag', dragged)
          .on('end', dragended);
      };

      const simulation = d3
        .forceSimulation(nodes)
        .force('link', d3.forceLink(links).id(d => d.id))
        .force('charge', d3.forceManyBody())
        .force('center', d3.forceCenter(width / 2, height / 2));

      // FIXME render using template
      const link = svg
        .append('g')
        .attr('stroke', '#999')
        .attr('stroke-opacity', 0.6)
        .selectAll('line')
        .data(links)
        .join('line')
        .attr('stroke-width', d => Math.sqrt(d.value));

      // FIXME render using template
      const node = svg
        .append('g')
        .attr('stroke', '#fff')
        .attr('stroke-width', 1.5)
        .selectAll('circle')
        .data(nodes)
        .join('circle')
        .attr('r', 5)
        .attr('fill', color)
        .call(drag(simulation));

      // FIXME
      simulation.on('tick', () => {
        link
          .attr('x1', d => d.source.x)
          .attr('y1', d => d.source.y)
          .attr('x2', d => d.target.x)
          .attr('y2', d => d.target.y);

        node.attr('cx', d => d.x).attr('cy', d => d.y);
      });

      return svg.node();
    }
  },

  watch: {
    data(val) {
      console.log('watch', 'data', val);
      this.$el.appendChild(this.chart(val));
    }
  }
};
</script>

<style lang="postcss" scoped>
.chart__container {
  @apply border border-gray-400 rounded;
}
</style>
