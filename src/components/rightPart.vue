<template>
    <div class="dropdown">
        <button class="dropbtn">{{displayName}}</button>
        <div class="dropdown-content">
            <button @click="cityChange('Beijing')">北京市</button>
            <button @click="cityChange('Shanghai')">上海市</button>
            <button @click="cityChange('Guangdong')">广东市</button>
            <button @click="cityChange('Chongqing')">重庆市</button>
        </div>
    </div>
    <div id="right_up">
          <LineChart :dataArr="dataLine"></LineChart>
        </div>
        <div id="right_down">
          <TimeLine :dataSc="dataSc" :dataBu="dataBu" :regionName="displayName"></TimeLine>
        </div>
</template>

<script>
import LineChart from "./LineChart.vue";
import TimeLine from "./TimeLine.vue";
export default {
    props: ['csvdata'],
  components: { LineChart, TimeLine },
  data() {
    return{
        mapName:{"Beijing":"北京市","Shanghai":"上海市","Guangdong":"广东市","Chongqing":"重庆市"},
        displayName:"北京市",
        dataLine:[],
        date:["20220131","20220228","20220331","20220430","20220531","20220630","20220731","20220831","20220930",
        "20221031","20221130","20221231"],
        dataSc:[],
        dataBu:[]
    }
  },
  methods:{
    cityChange(str){
        //清空
        // console.log(this.csvdata);
        this.displayName=this.mapName[str];
        this.dataSc.length=0;
        this.dataBu.length=0;
        this.dataLine.length=0;
        var i=0;
        for(;;i++){
            if(this.csvdata[i].RegionName==str && this.csvdata[i].Date=="20220101"){
                break;
            }
        }
        var j=0;
        for(;;i++){
            if(this.csvdata[i].Date=="20220101"||this.csvdata[i].Date=="20221231"){
                this.dataSc.push([this.csvdata[i].Date,this.csvdata[i]["C1M_School closing"]]);
                this.dataBu.push([this.csvdata[i].Date,this.csvdata[i]["C2M_Workplace closing"]]);
            }else {
                if(this.csvdata[i]["C1M_School closing"]!=this.csvdata[i-1]["C1M_School closing"]){
                    this.dataSc.push([this.csvdata[i].Date,this.csvdata[i]["C1M_School closing"]]);
                }
                if(this.csvdata[i]["C2M_Workplace closing"]!=this.csvdata[i-1]["C2M_Workplace closing"]){
                    this.dataBu.push([this.csvdata[i].Date,this.csvdata[i]["C2M_Workplace closing"]]);
                }
            }
            if(this.csvdata[i].Date==this.date[j]){
                this.dataLine.push(this.csvdata[i].ConfirmedCases);
                j++;
                if(j==this.date.length){
                    break;
                }
            }
        }
        // console.log(this.dataSc);
    }
    },
created(){
    // 装载数据
    this.cityChange("Beijing");
}

}
</script>

<style>
.dropdown {
    position: relative;
    display: inline-block;
}
.dropbtn {
    padding: 5px;
    font-size: 16px;
    border: none;
    cursor: pointer;
}
.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 62px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.1);
}
.dropdown-content button {
    color: black;
    padding:5px;
    font-size: 16px;
    text-decoration: none;
    display: block;
    text-align:center;
}
.dropdown-content button:hover {
    background-color: #a1c7e1;
    cursor: pointer;}
.dropdown:hover .dropdown-content {
    display: block;
    z-index: 2;
}
</style>