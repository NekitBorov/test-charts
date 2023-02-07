<template>
  <div>
    <div v-html="pageData.content" />
      <component
        v-for="component in components"
        :is="component.name"
        :ref="el => setComponents(component.id,el)"
        :key="component.id"
        :chart-options="component.options"
        @load="load(component)"
      />
<!--    <Chart ref="test" @load="load(Chart)" :chart-options="powerDemandChartOptions"/>-->
  </div>
</template>

<!--<script setup>-->
<!--import {defineAsyncComponent, ref} from 'vue'-->
<!--import Chart from "@/components/Chart.vue";-->

<!--const Chart1 = ref(null)-->
<!--const Chart2 = ref(null)-->

<!--const pageDataResponse = await fetch('https://demo.ccaf.io/cbeci/api/text_pages/cbsi%3Aeth_pos%3Aindex')-->
<!--const pageData = (await pageDataResponse.json()).data;-->

<!--const addFigure = (component) => {-->
<!--  const figures = [...document.getElementsByClassName('figure')]-->

<!--  console.log(Chart1.value.$el)-->

<!--  if (!figures.length) { return }-->

<!--  // figures[0].appendChild(test.value.$el)-->
<!--}-->

<!--const load = (component) => {-->
<!--  addFigure(component)-->
<!--}-->

<!--const powerDemandResponse = await fetch('https://demo.ccaf.io/cbeci/api/eth/pos/charts/network_power_demand')-->
<!--const powerDemandDataList = (await powerDemandResponse.json()).data;-->
<!--const powerDemandChartOptions = {-->
<!--  chart: {-->
<!--    renderTo: 'container'-->
<!--  },-->
<!--  title: {-->
<!--    text: 'Chart'-->
<!--  },-->
<!--  yAxis: {-->
<!--    title: {-->
<!--      text: 'Chart data'-->
<!--    }-->
<!--  },-->
<!--  series: [-->
<!--    {-->
<!--      name: 'guess_power',-->
<!--      data: powerDemandDataList.map(item => item.guess_power)-->
<!--    },-->
<!--    {-->
<!--      name: 'max_power',-->
<!--      data: powerDemandDataList.map(item => item.max_power)-->
<!--    },-->
<!--    {-->
<!--      name: 'min_power',-->
<!--      data: powerDemandDataList.map(item => item.min_power)-->
<!--    },-->
<!--  ]-->
<!--};-->

<!--const annConsumptionResponse = await fetch('https://demo.ccaf.io/cbeci/api/eth/pos/charts/annualised_consumption')-->
<!--const annConsumptionDataList = (await annConsumptionResponse.json()).data;-->
<!--const annConsumptionChartOptions = {-->
<!--  chart: {-->
<!--    renderTo: 'container'-->
<!--  },-->
<!--  title: {-->
<!--    text: 'Chart'-->
<!--  },-->
<!--  yAxis: {-->
<!--    title: {-->
<!--      text: 'Chart data'-->
<!--    }-->
<!--  },-->
<!--  series: [-->
<!--    {-->
<!--      name: 'guess_consumption',-->
<!--      data: annConsumptionDataList.map(item => item.guess_consumption)-->
<!--    },-->
<!--    {-->
<!--      name: 'max_consumption',-->
<!--      data: annConsumptionDataList.map(item => item.max_consumption)-->
<!--    },-->
<!--    {-->
<!--      name: 'min_consumption',-->
<!--      data: annConsumptionDataList.map(item => item.min_consumption)-->
<!--    },-->
<!--  ]-->
<!--};-->

<!--const comp = defineAsyncComponent(() => import('./Chart.vue'))-->

<!--const components = [-->
<!--    { name: comp, options: powerDemandChartOptions, id: 'Chart1'},-->
<!--    { name: comp, options: annConsumptionChartOptions, id: 'Chart2'}-->
<!--]-->

<!--</script>-->

<script>
import {defineAsyncComponent, ref} from 'vue'

export default {
  components: {
    Chart: defineAsyncComponent(() => import('./Chart.vue'))
  },
  async setup() {

    const Chart1 = ref(null)
    const Chart2 = ref(null)

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

    const components = [
      { name: 'Chart', options: powerDemandChartOptions, id: 'Chart1', queuePosition: 0 },
      { name: 'Chart', options: annConsumptionChartOptions, id: 'Chart2', queuePosition: 3}
    ]

    const comRefs = {};
    const setComponents = (key, el) => {
      comRefs[key] = el;
    }

    const addFigure = (component) => {
      const figures = [...document.getElementsByClassName('figure')]

      console.log()

      if (!figures.length) { return }

      figures[component.queuePosition].appendChild(comRefs[component.id].$el)
    }

    const load = (component) => {
      addFigure(component)
    }

    return {
      components,
      pageData,
      setComponents,
      addFigure,
      load
    }
  }
}
</script>

<style scoped>

</style>
