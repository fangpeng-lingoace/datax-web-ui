<template>
  <div class="dashboard-editor-container">
    <div><h2 class="welcome">Welcome {{username}} - 电子教室数据开发系统 </h2></div>
    <panel-group @handleSetLineChartData="handleSetLineChartData" />

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" />
    </el-row>
  </div>
</template>

<script>
import PanelGroup from './components/PanelGroup'
import LineChart from './components/LineChart'

import * as dashborad from '@/api/dashborad'

const lineChartData = {
  chartInfo: {
    failData: [],
    successData: [],
    dayList: []
  }
}

export default {
  name: 'DashboardAdmin',
  components: {
    PanelGroup,
    LineChart
  },
  data() {
    return {
      username: this.$store.getters.name,
      lineChartData: lineChartData.chartInfo
    }
  },
  created() {
    this.chartInfo()
  },
  methods: {
    handleSetLineChartData(type) {
      this.lineChartData = lineChartData[type]
    },
    chartInfo() {
      dashborad.chartInfo().then(response => {
        const { content } = response
        this.lineChartData.successData = content.triggerDayCountSucList
        this.lineChartData.failData = content.triggerDayCountFailList
        this.lineChartData.dayList = content.triggerDayList
        localStorage.setItem('countSucTotal', content.triggerCountSucTotal)
        localStorage.setItem('countRunningTotal', content.triggerCountRunningTotal)
        localStorage.setItem('countFailTotal', content.triggerCountFailTotal)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.welcome {
  color: gray;
}
.dashboard-editor-container {
  padding: 32px;
  background-color: rgb(240, 242, 245);
  position: relative;

  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
