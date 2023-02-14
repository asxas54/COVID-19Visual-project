

<template>
  <div class="background">
    <div class="header">中国COVID-19政府响应追踪数据可视化</div>
    <div class="grid-container">
      <div id="left_up" class="grid">
        <PieChart></PieChart>
      </div>
      <div id="middle_up" class="grid">
        <MapChart></MapChart>
      </div>
      <div id="right" class="grid">
        <div id="right_up">
          <LineChart :csvdata="csvdata()"></LineChart>
        </div>
        <div id="right_down">
          <TimeLine></TimeLine>
        </div>
      </div>
      <div id="left_down" class="grid">
        <ScatterChart></ScatterChart>
      </div>
      <div id="middle_down" class="grid">
        <BarChart></BarChart>
      </div>
    </div>
  </div>
</template>



<script >
import PieChart from "./components/PieChart.vue";
import ScatterChart from "./components/ScatterChart.vue";
import MapChart from "./components/MapChart.vue";
import BarChart from "./components/BarChart.vue";
import LineChart from "./components/LineChart.vue";
import TimeLine from "./components/TimeLine.vue";
import csv from './data/OxCGRT_CHN_latest - 2-6.csv'
// csv类型为object，this.csvdata()可以调用数据

// 导入子组件
export default {
  components: { PieChart, ScatterChart, MapChart, BarChart, LineChart, TimeLine },
  mounted() {
  },
  methods: {
    csvdata() {
      return csv;
    }
  }

};
</script>

<style>
html,body,#app{
  height: 100%;
  width: 100%;
}
.background{
  background-color: #222634;
  width: 100%;
  height: 100%;
}
.header{
  width: 100%;
  height: 10%;
  text-align: center;
  color:white;
  line-height: 60px;
  font-size: 30px;
}

.grid-container {
  width: 100%;
  height: 90%;
}
#left_up{
  width: 25%;
  height: 50%;
  left: 3%;
}
#middle_up{
  width: 40%;
  height: 50%;
  left: 5%;
}
#right{
  width: 25%;
  height: 94%;
  float: right;
  right: 2%;
}
#left_down{
  width: 25%;
  height: 40%;
  left: 3%;
  top: 4%;
}
#middle_down{
  width: 40%;
  height: 40%;
  left: 5%;
  top: 4%;
}
.grid{
  border: 2px solid white;
  border-radius: 25px;
  float: left;
  background-color: rgb(255, 255, 255, 0.1);
  position: relative;
  overflow: hidden;
}

</style>