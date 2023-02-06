<template>
  <div>
    <Chart :options="chartOptions"/>
  </div>
</template>

<script setup>
import { Chart } from 'highcharts-vue';
import { defineComponent } from "vue";

defineComponent({
  Chart
})

defineProps({
  chartOptions: {
    type: Object,
    default: () => {}
  }
})
</script>

<style scoped>

</style>
