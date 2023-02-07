<template>
  <div>
    <div v-html="pageData.content" />
      <component
        v-for="(component, i) in components"
        :is="component.name"
        :ref="el => componentRefs[component.type] = el"
        :key="component.type"
        :chart-type="component.type"
        @load="bindComponent(component)"
      />
  </div>
</template>

<script>
import {defineAsyncComponent, ref, reactive} from 'vue'

export default {
  components: {
    Chart: defineAsyncComponent(() => import('./Chart.vue'))
  },
  data () {
    return {
      components: []
    }
  },
  mounted () {
    // вернемся к реальной задаче. У нас есть N графиков(компонентов). И нам не нужно их всех рендерить на странице. А сколько нужно и какие говорит нам апи.
    // У каждого блока для подстановки(.figure) есть "number" - id графика. Давай узнаем какие графики нам нужны. Этот кусок кода в другом виде есть в примере
    [...document.getElementsByClassName('figure')].forEach((reservePlace, i) => {
      this.components.push({
        name: 'Chart',
        queuePosition: i,
        type: reservePlace.dataset.number,
      })
    });
  },
  async setup () {
    const pageDataResponse = await fetch('https://demo.ccaf.io/cbeci/api/text_pages/cbsi%3Aeth_pos%3Aindex');
    const pageData = (await pageDataResponse.json()).data;

    const componentRefs = {}; // хорошая была функция, но из-за одного присваивания наверно излишняя, оставил обьект

    const bindComponent = (component) => {
      const figures = document.getElementsByClassName('figure')  // оператор расширения для получения методов массива, обращение по индексу и так работает
      figures[component.queuePosition]?.appendChild(componentRefs[component.type].$el) // вместо проверки пустого массива "optional chaining"
    }; // удалим лишнюю функцию load и назовем функции по назначению

    return {
      pageData,
      componentRefs,
      bindComponent
    }
  }
}
</script>

<style scoped>

</style>
