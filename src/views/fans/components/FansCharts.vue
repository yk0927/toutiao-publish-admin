<template>
  <div class="fans-charts-container">
    <div ref="main" style="width: 600px; height: 400px"></div>
  </div>
</template>

<script>
import * as echarts from 'echarts'
export default {
  name: 'FansCharts',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () { },
  // 注意：初始化操作 DOM 一定要写到 mounted 钩子函数中
  mounted () {
    const mycharts = echarts.init(this.$refs.main)
    // 使用刚指定的配置项和数据显示图表
    mycharts.setOption({
      title: {
        text: 'ECharts 入门示例'
      },
      tooltip: {},
      xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
      },
      yAxis: {},
      series: [
        {
          name: '销量',
          type: 'bar',
          data: [5, 20, 36, 10, 10, 20]
        }
      ]
    })
  },
  methods: {}
}
</script>

<style lang="less" scoped></style>
