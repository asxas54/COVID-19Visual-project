

<template>
  <div class="background">
    <div class="header">中国COVID-19政府响应追踪数据可视化</div>
    <div class="grid-container">
      <div id="left_up" class="grid">
        <PieChart></PieChart>
      </div>
      <div id="middle_up" class="grid"></div>
      <div id="right_up" class="grid">
        <LineChart :csvdata="csvdata()"></LineChart>
      </div>
      <div id="left_down" class="grid"></div>
      <div id="middle_down" class="grid"></div>
    </div>
  </div>
</template>



<script >
import PieChart from "./components/PieChart.vue";
import LineChart from "./components/LineChart.vue";
// import { VueCsvImport } from 'vue-csv-import';
import csv from './data/OxCGRT_CHN_latest - 2-6.csv'
// 类型为object
// console.log(typeof csv);
// console.log(csv[1864]);
// console.log(csv[2228]);
// 导入子组件
export default {
  // setup() {
  //   const csvdata = csv;
  //   return {
  //     csvdata
  //   }
  // },
  components: { PieChart, LineChart },
  mounted() {
    // console.log(csv);
    // 1866 2230
    // console.log(this.csvdata());
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
#right_up{
  width: 25%;
  height: 94%;
  /* margin-right: 2%; */
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
  /* margin: 1%; */
  float: left;
  background-color: rgb(255, 255, 255, 0.1);
  position: relative;
  overflow: hidden;
}

</style>