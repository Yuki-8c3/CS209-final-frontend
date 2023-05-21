<template>
  <div id="chart-container" />
</template>

<script>
import echarts from 'echarts'
import 'echarts-wordcloud'
import axios from 'axios'

export default {
  name: 'FrequentlyDiscussedClasses',
  data() {
    return {
      chart: null
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
      axios.get('http://localhost:9090/FrequentlyDiscussedClasses')
        .then(response => {
          var keywords = []
          this.chart = echarts.init(document.getElementById('chart-container'))
          keywords = response.data
          console.log(response.data)
          var option = {
            title: {
              text: 'Classes frequently discussed WordCloud',
              textStyle: {
                fontStyle: 'oblique',
                fontSize: 40,
                color: '#201e65'
              },
              left: 'center',
              top: '15%' // 调整标题的位置，这里设置为距离顶部的百分比
            },
            tooltip: {},
            series: [{
              type: 'wordCloud',
              shape: {
                cloudGrow: 0.2
              },
              sizeRange: [10, 90],
              rotationRange: [-30, 30],
              gridSize: 2,
              drawOutOfBound: false,
              layoutAnimation: true,
              keepAspect: true,
              textStyle: {
                fontWeight: 'bold',
                color: function() {
                  return 'rgb(' + [
                    Math.round(Math.random() * 160),
                    Math.round(Math.random() * 160),
                    Math.round(Math.random() * 160)
                  ].join(',') + ')'
                }
              },
              emphasis: {
                textStyle: {
                  shadowBlur: 15,
                  shadowColor: '#333'
                }
              },
              data: keywords
            }]
          }

          this.chart.setOption(option)
          console.log(this.chart.option.series.data)
        })
        .catch(errorFrequentlyDiscussedClasses => {
          console.log('ERROR in FrequentlyDiscussedClasses')
        })
    }
  }
}
</script>
<style scoped>
#chart-container {
  height: 800px;
  width: 100%;
}
</style>
