<template>
  <div class="hello">
    <Chart :data="chartData" v-bind="{ width, height }"></Chart>
  </div>
</template>

<script>
import Chart from '@/components/Chart.vue';

export default {
  name: 'Home',

  props: {
    msg: String
  },

  components: {
    Chart
  },

  data() {
    return {
      chartData: [],
      width: 500,
      height: 200
    };
  },

  created() {
    this.getData().then(data => (this.chartData = data));
  },

  methods: {
    getData() {
      return Promise.resolve([99, 71, 78, 25, 36, 92]);
    }
  }
};
</script>
