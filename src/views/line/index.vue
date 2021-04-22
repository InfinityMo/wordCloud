<template>
  <div>
    <div class="line"
         id="chart"
         ref="charts">
    </div>

  </div>
</template>

<script>
import echarts from 'echarts'

import chartSettings from './chartSettings'
import select from './select'
export default {
  name: 'Home',
  data () {
    return {
      initChart: null,
      clouldChart: {
        width: 100,
        height: 400
      },
      fileName: '',
      selectData: select,
      excelData: [],
      inputSet: chartSettings,
      textBold: true
    }
  },
  computed: {
    chartStyle () {
      return { width: `${this.clouldChart.width}%`, height: `${this.clouldChart.height}px` }
    }
  },

  mounted () {
    this.$nextTick(() => {
      this.createChart()
    })
  },
  methods: {

    createChart () {
      this.initChart = echarts.init(this.$refs.charts)
      const chartData = [{
        name: '2月',
        value: 16758
      },
      {
        name: '3月',
        value: 15001
      },
      {
        name: '4月',
        value: 28932
      },
      {
        name: '5月',
        value: 36245
      },
      {
        name: '6月',
        value: 31563
      },
      {
        name: '7月',
        value: 36389
      },
      {
        name: '8月',
        value: 38000
      }
      ]
      const option = {
        backgroundColor: '#fff', // 背景色
        tooltip: {
          show: true,
          trigger: 'axis', // axis , item
          backgroundColor: 'rgba(0,15,78,0.6)',
          borderColor: '#00afff',
          borderWidth: 1,
          borderRadius: 0,
          textStyle: {
            color: '#fff',
            fontSize: 13,
            align: 'left'
          },
          axisPointer: {
            type: 'line', // 'line' | 'cross' | 'shadow' | 'none
            lineStyle: {
              width: 1,
              type: 'dotted',
              color: '#87CEEB'
            }
          }
        },
        legend: {
          show: false,
          orient: 'horizontal', // 'vertical'
          data: [],
          icon: 'circle',
          selectedMode: true,
          itemWidth: 10,
          itemHeight: 10,
          itemGap: 20,
          textStyle: {
            fontSize: 13,
            color: '#9bc8ff'
          },
          x: 'center',
          y: '25'
        },
        grid: {
          top: '25%',
          right: '3%',
          bottom: '10%',
          left: '10%'
        },
        xAxis: {
          type: 'category',
          axisLabel: {
            show: true,
            interval: 0, // 类目间隔 设置为 1，表示『隔一个标签显示一个标签』
            textStyle: {
              color: '#666',
              fontSize: 13
            },
            formatter: '{value}'
          },
          axisLine: {
            lineStyle: {
              color: '#D9D9D9'
            }
          },
          axisTick: {
            show: false // 坐标轴小标记
          },
          data: (function (data) {
            var arr = []
            data.forEach(function (items) {
              arr.push(items.name) // name
            })
            return arr
          })(chartData) // 载入横坐标数据
        },
        yAxis: {
          type: 'value',
          name: '（次）',
          nameTextStyle: {
            color: '#333',
            fontSize: 12,
            align: 'right'
          },
          axisLabel: {
            show: true,
            textStyle: {
              color: '#666',
              fontSize: 13
            },
            interval: 0, // 类目间隔 设置为 1，表示『隔一个标签显示一个标签』
            margin: 10
            // formatter: '{value}'
          },
          splitNumber: 5, // y轴刻度设置(值越大刻度越小)
          axisLine: { // y轴线
            show: true,
            lineStyle: {
              color: '#D9D9D9'
            }
          },
          splitLine: {
            show: false,
            lineStyle: {
              color: 'rgba(15,45,134,.6)', // 横向网格线颜色
              width: 1
            }
          },
          axisTick: {
            show: false // 坐标轴小标记
          }
        },
        series: [{
          name: '访问',
          type: 'scatter',
          stack: '总量',
          label: {
            normal: {
              show: false,
              position: 'top',
              textStyle: {
                color: '#9bc8ff',
                fontSize: 12
              },
              formatter: '{c}' // 图形上显示数字
            }
          },
          itemStyle: {
            normal: {
              color: '#87CEEB' // 颜色
            }
          },
          symbol: 'circle', // circle, rect, roundRect, triangle,  pin, diamond, arrow
          symbolPosition: 'end',
          symbolSize: 30,
          symbolOffset: [0, '-120%'],
          data: (function (data) {
            var arr = []
            data.forEach(function (items) {
              var itemName = items.name
              var itemValue = items.value
              var itemStyle = itemValue / 1000 // console.log(itemStyle)
              arr.push({
                name: itemName,
                value: itemValue,
                symbolSize: itemStyle
              })
            })
            return arr
          })(chartData) // 载入数据并设置图形尺寸
        }

        ]
      }
      this.initChart.setOption(option)
    },
    switchChange (value) {
      value ? this.inputSet.normal.fontWeight = 'bold' : this.inputSet.normal.fontWeight = 'normal'
    }
  }
}
</script>
<style lang="less" scoped>
// @import "./index.less";
.line {
  width: 100%;
  height: 500px;
}
</style>
