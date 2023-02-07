<template>
  <div>
    <Chart :options="chartOptions"/>
  </div>
</template>

<script setup>
import { Chart } from 'highcharts-vue';
import { onMounted } from "vue";

const emit = defineEmits(['load'])

defineProps({
  chartOptions: {
    type: Object,
    default: () => {}
  }
})

onMounted(() => emit('load'))
</script>

<style scoped>

</style>
