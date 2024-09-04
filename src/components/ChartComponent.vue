<script setup>
import { ref, watch, nextTick } from 'vue'
import Highcharts from 'highcharts'

const props = defineProps({
  data: {
    type: Array,
    default: () => []
  }
})

const chartContainer = ref(null)

watch(
  () => props.data,
  async (newData) => {
    if (newData.length) {
      await nextTick()
      if (chartContainer.value) {
        Highcharts.chart(chartContainer.value, {
          chart: {
            type: 'line'
          },
          title: {
            text: null
          },
          xAxis: {
            categories: ['Текущий день', 'Вчера', 'Это день недели'],
            labels: {
              enabled: false // Отключаем метки на оси X
            },
            tickLength: 5,
            lineWidth: 1
          },
          yAxis: {
            title: {
              text: null
            },
            labels: {
              enabled: true,
              formatter: function () {
                return '▪' // Используем квадрат вместо кружка
              },
              align: 'right',
              x: 5,
              y: 8,
              style: {
                fontSize: '24px',
                fontWeight: 'bold'
              }
            },
            tickPositioner: function () {
              return
            },
            tickLength: 0,
            gridLineWidth: 0,
            lineWidth: 1
          },
          legend: {
            enabled: false
          },
          credits: {
            enabled: false
          },
          series: [
            {
              name: '',
              data: newData,
              color: '#3f7d66'
            }
          ]
        })
      }
    }
  },
  { immediate: true }
)
</script>

<template>
  <div ref="chartContainer"></div>
</template>

<style scoped>
#chartContainer {
  width: 100%;
  height: 400px;
}
</style>
