<template>
  <div :id="id" :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
// import resize from './mixins/resize'
import '@/../node_modules/echarts/map/js/province/shandong.js' // 引入山东地图数据
// json 数据来源   http://datav.aliyun.com/tools/atlas/index.html#&lat=30.332329214580188&lng=106.72278672066881&zoom=3.5
import shandong from './shandong.json'
export default {
  // mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    id: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '200px'
    },
    height: {
      type: String,
      default: '200px'
    }
  },
  data() {
    return {
      chart: null,
      dataJson: [
        { 'name': '临沂市', 'value': 17192, 'rank': 'TOP1' },
        { 'name': '潍坊市', 'value': 16138, 'rank': 'TOP2' },
        { 'name': '烟台市', 'value': 13654, 'rank': 'TOP3' },
        { 'name': '菏泽市', 'value': 12256, 'rank': 'TOP4' },
        { 'name': '济宁市', 'value': 11191, 'rank': 'TOP5' },
        { 'name': '青岛市', 'value': 11064, 'rank': 'TOP6' },
        { 'name': '德州市', 'value': 10361, 'rank': 'TOP7' },
        { 'name': '滨州市', 'value': 9156, 'rank': 'TOP8' },
        { 'name': '聊城市', 'value': 8721, 'rank': 'TOP9' },
        { 'name': '东营市', 'value': 8617, 'rank': 'TOP10' },
        { 'name': '济南市', 'value': 8177, 'rank': 'TOP11' },
        { 'name': '泰安市', 'value': 7762, 'rank': 'TOP12' },
        { 'name': '淄博市', 'value': 5964, 'rank': 'TOP13' },
        { 'name': '威海市', 'value': 5797, 'rank': 'TOP14' },
        { 'name': '日照市', 'value': 5359, 'rank': 'TOP15' },
        { 'name': '枣庄市', 'value': 4564, 'rank': 'TOP16' },
        { 'name': '莱芜市', 'value': 2246, 'rank': 'TOP17' }
      ],
      markPointData: [
        [{ 'value': [118.286421, 35.311899] }], // 临沂
        [{ 'value': [119.077723, 36.554349] }] // 潍坊
      ]
    }
  },
  mounted() {
    this.initChart()
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      debugger
      // 初始化地图容器
      var MapChart = echarts.init(document.getElementById(this.id))
      echarts.registerMap('shandong', shandong)
      var option = {
        tooltip: {
          // 显示悬浮窗口
          trigger: 'item',
          backgroundColor: 'rgba(26,26,26)',
          formatter: function(params) {
            // 定义一个res变量来保存最终返回的字符结果,并且先把地区名称放到里面
            var res = params.name + '    '
            // 定义一个变量来保存series数据系列
            var myseries = option.series

            // 循环遍历series数据系列
            for (let i = 0; i < myseries.length; i++) {
              // 在内部继续循环series[i],从data中判断：当地区名称等于params.name的时候就将当            前数据和名称添加到res中供显示

              if (myseries[i].data) {
                for (let k = 0; k < myseries[i].data.length; k++) {
                  // console.log(myseries[i].data[k].name);
                  // 如果data数据中的name和地区名称一样
                  if (myseries[i].data[k].name === params.name) {
                    // 将series数据系列每一项中的name和数据系列中当前地区的数据添加到res中

                    res +=
                      '<span style="color:#FFCC00">' +
                      myseries[i].data[k].rank +
                      '</span><br />' +
                      myseries[i].name +
                      ' :   ' +
                      myseries[i].data[k].value +
                      '平方千米' +
                      '<br />'
                  }
                }
              }
            }

            // 返回res
            // console.log(res);
            return res
          }
        },
        label: {
          show: true,
          emphasis: {
            show: true
          }
        },
        animation: true,
        visualMap: {
          min: 0,
          left: 10,
          type: 'piecewise',
          bottom: 70,
          textStyle: {
            color: '#999'
          },
          splitNumber: 6,
          seriesIndex: [0],
          itemWidth: 20, // 每个图元的宽度
          itemGap: 5, // 图元之间的距离
          pieces: [
            {
              gt: 15000,
              color: '#007EFF',
              label: '15000平方千米以上'
            },
            {
              gte: 12000,
              lte: 15000,
              color: '#3BB7FF',
              label: '12000-15000平方千米'
            },
            {
              gte: 10000,
              lte: 12000,
              color: '#64C6FF',
              label: '10000-12000平方千米'
            },
            {
              gte: 8000,
              lte: 10000,
              color: '#92D7FF',
              label: '8000-10000平方千米'
            },
            {
              gte: 5000,
              lte: 8000,
              color: '#C9EBFF',
              label: '5000-8000平方千米'
            },
            {
              lte: 5000,
              color: '#E9FBFF',
              label: '5000平方千米以下'
            }
          ]
        },
        geo: {
          top: 0,
          roam: !1,
          scaleLimit: {
            min: 1,
            max: 4
          },
          zoom: 1,
          label: {
            normal: {
              show: false,
              fontSize: '14',
              color: 'red'
            }
          },
          itemStyle: {
            normal: {
              shadowBlur: 4,
              shadowColor: '#fff',
              borderColor: '#fff'
            },
            emphasis: { // 鼠标经过
              areaColor: 'rgba(0,126,255,0.6)',
              shadowOffsetX: 0,
              shadowOffsetY: 0,
              borderWidth: 2,
              borderColor: '#007EFF'
            }
          },
          map: '山东'
        },

        series: [
          {
            name: '占地面积',
            type: 'map',
            geoIndex: 0,
            data: this.dataJson,
            itemStyle: {
              normal: {
                label: {
                  show: false
                },
                areaColor: 'orange'
              }
            }
          },
          {
            type: 'scatter',
            coordinateSystem: 'geo',
            data: this.markPointData[0],
            symbol:
              'path://M896 40H128c-17.7 0-32 14.3-32 32s14.3 32 32 32h32v857.6c0 28.5 33.2 46.2 59.2 31.5L478 847.2c20.9-11.9 47.2-11.9 68.1 0l258.8 145.9c26 14.7 59.2-2.9 59.2-31.5V104h32c17.7 0 32-14.3 32-32S913.7 40 896 40z   M573.8 208.4V694h-74.1V297.5c-29.9 28.1-67.1 48.5-111.5 61.2v-73.4c21.8-5.4 44.9-15 69.4-28.6 24.5-15.4 44.4-31.5 59.8-48.3h56.4z',
            symbolSize: 18,
            itemStyle: {
              normal: {
                color: '#fcc302'
              }
            },
            label: {
              normal: {
                show: true,
                formatter: function(params) {
                  return params.name
                },
                position: 'bottom',
                color: 'red',
                fontSize: '16'
              },
              emphasis: {
                show: false
                // color: '#00d5e9',
              }
            }
          },
          {
            type: 'scatter',
            coordinateSystem: 'geo',
            data: this.markPointData[1],
            symbol:
              'path://M634.4 247.4c29.5 27.2 44.2 62.1 44.2 104.7 0 41.3-15.9 79.6-47.6 114.9-19 19.5-52.6 45.6-100.6 78.2-52.1 34.5-83.4 65.1-93.8 91.8h242.8v65.3H344.7c0-48.5 16.1-90.9 48.3-127.2 16.8-19.9 53.3-49.4 109.5-88.4 33.1-23.6 56.2-42.2 69.4-55.8 22.2-24.9 33.3-51.4 33.3-79.6 0-27.2-7.5-47.6-22.4-61.2s-37-20.4-66-20.4c-30.8 0-54 10.4-69.4 31.3-15.9 20-24.5 49.6-25.8 89.1h-74.1c0.9-54.4 16.5-97.9 46.9-130.6 30.8-34.9 72.5-52.4 125.1-52.4 47.1 0.2 85.4 13.6 114.9 40.3z  M896 40H128c-17.7 0-32 14.3-32 32s14.3 32 32 32h32v857.6c0 28.5 33.2 46.2 59.2 31.5L478 847.2c20.9-11.9 47.2-11.9 68.1 0l258.8 145.9c26 14.7 59.2-2.9 59.2-31.5V104h32c17.7 0 32-14.3 32-32S913.7 40 896 40z ',
            symbolSize: 18,
            itemStyle: {
              normal: {
                color: '#FFCC33'
              }
            },
            label: {
              normal: {
                show: true,
                formatter: function(params) {
                  return params.name
                },
                position: 'bottom',
                color: 'red',
                fontSize: '11'
              },
              emphasis: {
                show: true,
                color: '#00d5e9'
              }
            }
          }
        ]
      }
      MapChart.setOption(option)
      setTimeout(function() {
        // 可以同时生效
        window.addEventListener('resize', () => {
          MapChart.resize()
        })
      }, 200)
    }
  }
}
</script>
