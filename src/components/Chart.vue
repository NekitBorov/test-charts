<template>
  <div>
    <Chart :options="chartOptions"/>
  </div>
</template>

<script>
import { Chart } from 'highcharts-vue';

export default {
  components: {
    Chart
  },
  props: {
    chartOptions: {
      type: Object,
      default: () => {}
    }
  },
  mounted() {
    this.$emit('load')
  }
}
</script>

<style scoped>

</style>
