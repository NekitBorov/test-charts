<template>
  <div v-if="chartOptions.series">
    <Chart :options="chartOptions"/>
  </div>
</template>

<script>
import { Chart } from 'highcharts-vue';
import loadChartData from '../mixins/chartData.js'

export default {
  components: {
    Chart
  },
  props: {
    chartType: {  // тут передадим только тип/id графика, но тут все графики, даже которых у нас нет, но мы их скроем выше
      type: String,
      default: ""
    }
  },
  data () {
    return {
      chartOptions: {}
    }
  },
  mixins: [loadChartData],  // тут создадим библиотеку графиков
  mounted () {
    this.$emit('load');
  }
}
</script>

<style scoped>

</style>
