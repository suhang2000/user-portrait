<template>
  <div id="political_status" :data="data" style="width: 65vw;height: 65vh;"></div>
</template>

<script>
export default {
  name: 'PoliticalStatus',
  data () {
    return {
      data: []
    }
  },
  methods: {
    initChart () {
      this.chart = this.$echarts.init(document.getElementById('political_status'))
      const option = {
        title: {
          text: '政治面貌分布',
          left: 'center'
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          orient: 'vertical',
          left: 'left'
        },
        series: [
          {
            name: '政治面貌分布',
            type: 'pie',
            radius: '50%',
            data: this.data,
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
      this.chart.setOption(option)
      window.onresize = this.chart.resize
    },
    loadData () {
      this.$axios.get('/personal/politicalStatus').then(resp => {
        if (resp.data.code === 200) {
          this.data = resp.data.data
          // console.log(this.ageData)
          this.initChart()
        } else {
          this.$alert(resp.data.message, '提示', {
            confirmButtonText: '确定'
          })
        }
      }).catch(failResponse => {
        this.$message('加载失败')
      })
    }
  },
  mounted () {
    this.loadData()
  }
}
</script>

<style scoped>

</style>
