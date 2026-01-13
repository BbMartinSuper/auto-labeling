<template>
  <a-card title="声音类型分布" :bordered="true" class="chart-card">
    <v-chart class="chart" :option="chartOption" autoresize />
  </a-card>
</template>

<script setup>
import { computed } from 'vue'
import VChart from 'vue-echarts'
import { use } from 'echarts/core'
import { BarChart } from 'echarts/charts'
import {
  TitleComponent,
  TooltipComponent,
  GridComponent
} from 'echarts/components'
import { CanvasRenderer } from 'echarts/renderers'

use([
  BarChart,
  TitleComponent,
  TooltipComponent,
  GridComponent,
  CanvasRenderer
])

const props = defineProps({
  soundTypes: {
    type: Array,
    required: true
  }
})

const chartOption = computed(() => {
  if (!props.soundTypes || props.soundTypes.length === 0) {
    return {
      title: {
        text: '加载中...',
        left: 'center',
        top: 'center'
      }
    }
  }

  const data = props.soundTypes[0]
  const categories = Object.keys(data)
  const values = Object.values(data).map(value => parseInt(value))

  return {
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'shadow'
      },
      formatter: '{b}: {c}%'
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      containLabel: true
    },
    xAxis: {
      type: 'category',
      data: categories,
      axisLabel: {
        interval: 0,
        rotate: 0
      }
    },
    yAxis: {
      type: 'value',
      max: 100,
      axisLabel: {
        formatter: '{value}%'
      }
    },
    series: [
      {
        name: '声音类型',
        type: 'bar',
        barWidth: '50%',
        data: values,
        itemStyle: {
          borderRadius: [5, 5, 0, 0],
          color: function(params) {
            const colors = ['#5470c6', '#91cc75', '#fac858', '#ee6666']
            return colors[params.dataIndex % colors.length]
          }
        },
        label: {
          show: true,
          position: 'top',
          formatter: '{c}%'
        }
      }
    ]
  }
})
</script>

<style scoped>
.chart-card {
  height: 100%;
}

.chart {
  height: 400px;
  width: 100%;
}
</style>