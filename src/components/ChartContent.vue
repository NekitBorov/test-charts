<template>
  <Chart :chart-options="powerDemandChartOptions" />
  <Chart :chart-options="annConsumptionChartOptions" />
</template>

<script setup>
import Chart from './Chart.vue'

const powerDemandResponse = await fetch('https://demo.ccaf.io/cbeci/api/eth/pos/charts/network_power_demand')
const powerDemandDataList = (await powerDemandResponse.json()).data;
const powerDemandChartOptions = {
  chart: {
    renderTo: 'container'
  },
  title: {
    text: 'Chart'
  },
  yAxis: {
    title: {
      text: 'Chart data'
    }
  },
  series: [
    {
      name: 'guess_power',
      data: powerDemandDataList.map(item => item.guess_power)
    },
    {
      name: 'max_power',
      data: powerDemandDataList.map(item => item.max_power)
    },
    {
      name: 'min_power',
      data: powerDemandDataList.map(item => item.min_power)
    },
  ]
};

const annConsumptionResponse = await fetch('https://demo.ccaf.io/cbeci/api/eth/pos/charts/annualised_consumption')
const annConsumptionDataList = (await annConsumptionResponse.json()).data;
const annConsumptionChartOptions = {
  chart: {
    renderTo: 'container'
  },
  title: {
    text: 'Chart'
  },
  yAxis: {
    title: {
      text: 'Chart data'
    }
  },
  series: [
    {
      name: 'guess_consumption',
      data: annConsumptionDataList.map(item => item.guess_consumption)
    },
    {
      name: 'max_consumption',
      data: annConsumptionDataList.map(item => item.max_consumption)
    },
    {
      name: 'min_consumption',
      data: annConsumptionDataList.map(item => item.min_consumption)
    },
  ]
};
</script>

<style scoped>

</style>
