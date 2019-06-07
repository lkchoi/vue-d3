<template>
  <div class="hello">
    <div class="container bg-gray-100 p-8 rounded">
      <h3 class="text-lg">Force Directed Graph</h3>
      <ForceDirectedGraph :data="chartData" v-bind="{ width, height }" />
    </div>
  </div>
</template>

<script>
import ForceDirectedGraph from '@/components/ForceDirectedGraph.vue';

export default {
  name: 'Home',

  props: {
    msg: String
  },

  components: {
    ForceDirectedGraph
  },

  data() {
    return {
      chartData: {
        links: [],
        nodes: []
      },
      width: 1024,
      height: 600
    };
  },

  async created() {
    this.chartData = await this.getData();
  },

  methods: {
    async getData() {
      return (await fetch('/data/miserables.json')).json();
    }
  }
};
</script>
