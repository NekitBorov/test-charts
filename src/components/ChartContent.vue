<script setup>
  import { ref, onUnmounted, getCurrentInstance, defineAsyncComponent } from 'vue'
  import renderComponent from './renderComponent'
  const { appContext } = getCurrentInstance()
  const container = ref()
  let destroyComp = null

  const component = defineAsyncComponent(() => import('./Chart.vue'))

  const pageDataResponse = await fetch('https://demo.ccaf.io/cbeci/api/text_pages/cbsi%3Aeth_pos%3Aindex')
  const pageData = (await pageDataResponse.json()).data;

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
  const components = [{ value: 'chart', options: powerDemandChartOptions, id: 'chart1'}]

  onUnmounted(() => destroyComp?.())

  const load = async () => {
    destroyComp?.()
    destroyComp = renderComponent({
      el: document.getElementsByClassName('figure')[2],
      component: component,
      props: {
        'chart-options': components[0].options
      },
      appContext,
    })
  }

  // , { value: Chart, options: annConsumptionChartOptions, id: 'Chart2'}
</script>

<template>
  <div>
    <div v-html="pageData.content" ref="container" id="container" @click="load" />
    <component
            :is="component"
            @load="load"
    />
  </div>
</template>

<style scoped>
  .loaded {
  }
</style>
