<template>
  <div>
    <div v-html="pageData.content" />
    <component
        v-for="component in components"
        :is="component.value"
        ref="Chart1"
        :key="component.id"
        :chart-options="component.options"
        @load="load(component)"
    />
<!--    <Chart :chart-options="powerDemandChartOptions" />-->
<!--    <Chart :chart-options="annConsumptionChartOptions" />-->
  </div>
</template>

<script setup>
import Chart from './Chart.vue'
import {ref, onMounted } from 'vue'

const Chart1 = ref(null)
const Chart2 = ref(null)
const test = ref(null)

onMounted(() => console.log('test.value',test.value))

const refs = [Chart1, Chart2]

const pageDataResponse = await fetch('https://demo.ccaf.io/cbeci/api/text_pages/cbsi%3Aeth_pos%3Aindex')
const pageData = (await pageDataResponse.json()).data;

const addFigure = (component) => {
  const figures = [...document.getElementsByClassName('figure')]
  if (!figures.length) { return }

  console.log('refs', Chart1.value)

  if (refs[component.id]) {
    refs[component.id][0].$el.style.display = 'flex'
    refs[component.id][0].$el.style.display = 'block'
    figures[component.order].appendChild(refs[component.id][0].$el)
  }
}

const load = (component) => {
  addFigure(component)
}

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

// , { value: Chart, options: annConsumptionChartOptions, id: 'Chart2'}
const components = [{ value: Chart, options: powerDemandChartOptions, id: 'Chart1'}]
</script>

<style scoped>

</style>
