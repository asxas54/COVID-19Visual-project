<template>
  <!-- <div class> -->
  <canvas id="title" width="360" height="60" ></canvas>
  <canvas id="school" width="360" height="80"></canvas>
  <canvas id="work" width="360" height="80"></canvas>
  <!-- </div> -->
  <!-- width="360" height="80" -->
</template>

<style>
canvas {
  /*border: 1px solid black;*/
  margin: 10px auto;
  display: block;
  /*background-color: #AAAAAA;*/
}
</style>

<script>
export default {
    props: ['dataSc','dataBu','regionName'],
    methods:{
      drawCanvas(){
        var canvas_title = document.getElementById("title");
        var ctx_title = canvas_title.getContext("2d");

        var getPixelRatio = function (context){
            var devicePixelRatio = window.devicePixelRatio || 1;
            var backingStoreRatio = ctx_title.webkitBackingStorePixelRatio ||
                ctx_title.mozBackingStorePixelRatio ||
                ctx_title.msBackingStorePixelRatio ||
                ctx_title.oBackingStorePixelRatio ||
                ctx_title.backingStorePixelRatio || 1;
            console.log(devicePixelRatio / backingStoreRatio);
            return (devicePixelRatio / backingStoreRatio);
        };

        var radio = 1;//getPixelRatio(ctx_title);
        // console.log(radio);
        canvas_title.style.width = canvas_title.width + 'px';
        canvas_title.style.height = canvas_title.height + 'px';
        canvas_title.width = window.innerWidth/3; //canvas_title.width * radio;
        canvas_title.height =  80*2;//canvas_title.height * radio;
        canvas_title.style.height = canvas_title.height/2 + "px";
        canvas_title.style.width = canvas_title.width/2 + "px";

      // 画标题
        ctx_title.font = "bold 36px Arial";
        ctx_title.textAlign = "center";
        ctx_title.fillStyle = "white";
        ctx_title.fillText("2022年政策追踪", canvas_title.width / 2, 30);
        // 北京市

      // 颜色提示
      function drawNotice(ctx_title,x0,y0,x1,x2,y2,color,str){
        ctx_title.beginPath();
        ctx_title.moveTo(x0, y0);
        ctx_title.lineTo(x1, y0);
        ctx_title.strokeStyle = color;
        ctx_title.lineWidth = 12;
        ctx_title.font = "22px Arial";
        ctx_title.fillText(str, x2, y2);
        ctx_title.stroke();

      }
      var lineH=30,lineW=60,rightW=90,lineR=240,textL=140;
      drawNotice(ctx_title,0,65,lineW,textL,80,"#66CDAA","等级0，无措施");
      drawNotice(ctx_title,0,65+lineH,lineW,textL+13,80+lineH,"#FFEC8B","等级1，建议措施");
      drawNotice(ctx_title,canvas_title.width-lineR,65,canvas_title.width-lineR+lineW,canvas_title.width-rightW, 80,"#FF7F24","等级2，部分强制");
      drawNotice(ctx_title,canvas_title.width-lineR,65+lineH,canvas_title.width-lineR+lineW,canvas_title.width-rightW, 80+lineH,"#FF3030","等级3，完全强制");
        // ctx_title.beginPath();
        // ctx_title.moveTo(0, 65);
        // ctx_title.lineTo(60, 65);
        // ctx_title.strokeStyle = "#66CDAA";
        // ctx_title.lineWidth = 10;
        // ctx_title.font = "26px Arial";
        // ctx_title.fillText("等级0，无措施", 120, 80);
        // ctx_title.stroke();

        // ctx_title.beginPath();
        // ctx_title.moveTo(0, 55+20);
        // ctx_title.lineTo(30, 55+20);
        // ctx_title.strokeStyle = "#FFEC8B";
        // ctx_title.lineWidth = 4;
        // ctx_title.font = "26px Arial";
        // ctx_title.fillText("等级1，建议措施", 107, 80+20);
        // ctx_title.stroke();

        // ctx_title.beginPath();
        // ctx_title.moveTo(canvas_title.width-145, 55);
        // ctx_title.lineTo(canvas_title.width-115, 55);
        // ctx_title.strokeStyle = "#FF7F24";
        // ctx_title.lineWidth = 4;
        // ctx_title.font = "26px Arial";
        // ctx_title.fillText("等级2，部分强制", canvas_title.width-80, 80);
        // ctx_title.stroke();

        // ctx_title.beginPath();
        // ctx_title.moveTo(canvas_title.width-145, 55+20);
        // ctx_title.lineTo(canvas_title.width-115, 55+20);
        // ctx_title.strokeStyle = "#FF3030";
        // ctx_title.lineWidth = 4;
        // ctx_title.font = "26px Arial";
        // ctx_title.fillText("等级3，完全强制", canvas_title.width-80, 80+20);
        // ctx_title.stroke();

        const canvas_school = document.getElementById("school");
        const ctx_school = canvas_school.getContext("2d");

        // canvas_school.style.width = canvas_school.width + 'px';
        // canvas_school.style.height = canvas_school.height + 'px';
        // canvas_school.width = canvas_school.width * radio;
        // canvas_school.height = canvas_school.height * radio;
        canvas_school.width = window.innerWidth/3+200; //canvas_title.width * radio;
        canvas_school.height =  80*2;//canvas_title.height * radio;
        canvas_school.style.height = canvas_school.height/2 + "px";
        canvas_school.style.width = canvas_school.width/2 + "px";

        // 直线含义
        var lineT=60;
        ctx_school.font = "30px Arial";
        ctx_school.fillStyle = "white";
        ctx_school.fillText("学校关闭政策", 0, lineT);
        
        function strtoDay(str){
          // console.log(str);
          if(str=="20221231"){return 1;}
          else{
            var ans=(((str[4]-'0')*10+(str[5]-'0')-1)*30+((str[6]-'0')*10+(str[7]-'0'))-1)/365;
            // console.log(ans);
            return ans;}
        }

        var mapColor={0:"#66CDAA",1:"#FFEC8B",2:"#FF7F24",3:"#FF3030"};

        function drawLine(ctx_school,x1,y1,x2,y2,color){
          ctx_school.beginPath();
          ctx_school.moveTo(x1,y1);
          ctx_school.lineTo(x2,y2);
          ctx_school.strokeStyle = color;
          ctx_school.lineWidth = 6;
          ctx_school.stroke();
        }

        function drawPoint(ctx_school,x,y,r,color){
          ctx_school.beginPath();
          ctx_school.arc(x, y, r, 0, 2 * Math.PI, false);
          ctx_school.fillStyle = color;
          ctx_school.fill();
        }

        function drawArc(ctx_school,x,y,r,color){
          ctx_school.beginPath();
          ctx_school.arc(x, y, r, 0, 2 * Math.PI, false);
          ctx_school.lineWidth = 2;
          ctx_school.strokeStyle = color;
          ctx_school.stroke();
        }
        // console.log(this.dataSc);
        // 画白色圈
        function drawTimeLine(ctx_school,beginx,drawW,beginy,dataSc){
          // var beginx=110,drawW=canvas_school.width-beginx,beginy=50;
        for(var i=0;i<dataSc.length-1;i++){
          var x1=beginx+strtoDay(dataSc[i][0])*drawW;
          var x2=beginx+strtoDay(dataSc[i+1][0])*drawW;
          var drawC=mapColor[dataSc[i][1]];

          drawLine(ctx_school,x1,beginy,x2,beginy,drawC);
          drawPoint(ctx_school,x1,beginy,12,"#8B8378");
          drawArc(ctx_school,x1,beginy,8,"white");
        }
        }
        var leftW=220;
        drawTimeLine(ctx_school,leftW,canvas_school.width-leftW,lineT,this.dataSc);
        //       // 画直线1.1
        // ctx_school.beginPath();
        // ctx_school.moveTo(110, 50);
        // ctx_school.lineTo((canvas_school.width-110)/4+110, 50);
        // ctx_school.strokeStyle = "yellow";
        // ctx_school.lineWidth = 3;
        // ctx_school.stroke();

        // // 画直线1.2
        // ctx_school.beginPath();
        // ctx_school.moveTo((canvas_school.width-110)/4+110, 50);
        // ctx_school.lineTo(canvas_school.width, 50);
        // ctx_school.strokeStyle = "red";
        // ctx_school.stroke();

        // 画空心小圆-20220101
        // 为了擦除空心小圆里面的直线，需要先把空心小圆包含的区域涂成背景色（一般是白色），再重新画一个空心小圆

        
        
        
        // ctx_school.beginPath();
        // ctx_school.arc(110, 50, 4, 0, 2 * Math.PI, false);
        

        // 画空心小圆-20220314大概四分之一
        // ctx_school.beginPath();
        // ctx_school.arc((canvas_school.width-110)/4+110, 50, 6, 0, 2 * Math.PI, false);
        // ctx_school.fill();

        // ctx_school.beginPath();
        // ctx_school.arc((canvas_school.width-110)/4+110, 50, 4, 0, 2 * Math.PI, false);
        // ctx_school.stroke();
        if(this.regionName=="北京市"){
          // 画竖直的16px的直线
          
          ctx_school.beginPath();
          ctx_school.moveTo(leftW, lineT+4);
          ctx_school.lineTo(leftW, lineT+30);
          ctx_school.stroke();

          ctx_school.beginPath();
          ctx_school.moveTo((canvas_school.width-leftW)*0.2+leftW, lineT-4);
          ctx_school.lineTo((canvas_school.width-leftW)*0.2+leftW, lineT-30);
          ctx_school.stroke();

          //政策调整
          ctx_school.font = "24px Arial";
          ctx_school.fillStyle = "white";
          ctx_school.fillText("1月1日学校建议关闭", 60, 120);

          ctx_school.fillText("3月14日某些级别学校要求关闭", (canvas_school.width-110)/4+30, 20);
        }

        
      
        const canvas_work = document.getElementById("work");
        const ctx_work = canvas_work.getContext("2d");

        canvas_work.width = window.innerWidth/3+200;
        canvas_work.height = 80*2 ;
        canvas_work.style.width = canvas_work.width/2 + 'px';
        canvas_work.style.height = canvas_work.height/2 + 'px';
        
        // 直线含义
        ctx_work.font = "30px Arial";
        ctx_work.fillStyle = "white";
        ctx_work.fillText("办公场所政策", 0, lineT);

        // console.log(this.dataBu);
        drawTimeLine(ctx_work,leftW,canvas_work.width-leftW,lineT,this.dataBu);

        // 画直线1.1
        // ctx_work.beginPath();
        // ctx_work.moveTo(110, 50);
        // // ctx_work.lineTo((canvas_work.width-110)/4+110, 50);
        // ctx_work.lineTo(canvas_work.width-(canvas_work.width-110)/12, 50);
        // ctx_work.strokeStyle = "red";
        // ctx_work.lineWidth = 3;
        // ctx_work.stroke();

        // // 画直线1.2
        // ctx_work.beginPath();
        // ctx_work.moveTo(canvas_work.width-(canvas_work.width-110)/12, 50);
        // ctx_work.lineTo(canvas_work.width, 50);
        // ctx_work.strokeStyle = "yellow";
        // ctx_work.stroke();

        // // 画空心小圆-20220101
        // // 为了擦除空心小圆里面的直线，需要先把空心小圆包含的区域涂成背景色（一般是白色），再重新画一个空心小圆
        // ctx_work.beginPath();
        // ctx_work.arc(110, 50, 6, 0, 2 * Math.PI, false);
        // ctx_work.fillStyle = "black";
        // ctx_work.fill();

        // ctx_work.beginPath();
        // ctx_work.arc(110, 50, 4, 0, 2 * Math.PI, false);
        // ctx_work.lineWidth = 1;
        // ctx_work.strokeStyle = "white";
        // ctx_work.stroke();

        // // 画空心小圆-20220314大概四分之一
        // ctx_work.beginPath();
        // ctx_work.arc(canvas_work.width-(canvas_work.width-110)/12, 50, 6, 0, 2 * Math.PI, false);
        // ctx_work.fill();

        // ctx_work.beginPath();
        // ctx_work.arc(canvas_work.width-(canvas_work.width-110)/12, 50, 4, 0, 2 * Math.PI, false);
        // ctx_work.stroke();

        // console.log(this.regionName);
        if(this.regionName=="北京市"){
          // 画竖直的16px的直线
          ctx_work.beginPath();
          ctx_work.moveTo(leftW, lineT+4);
          ctx_work.lineTo(leftW, lineT+30);
          ctx_work.stroke();

          ctx_work.beginPath();
          var xt=110+strtoDay("20221222")*(canvas_work.width-110);
          ctx_work.moveTo(xt, lineT-4);
          ctx_work.lineTo(xt, lineT-30);
          ctx_work.stroke();

          //政策调整
          ctx_work.font = "24px Arial";
          ctx_work.fillStyle = "white";
          ctx_work.fillText("1月1日部分岗位在家办公", 60, 120);

          ctx_work.fillText("12月22日建议在家办公", canvas_work.width-(canvas_work.width-110)/12-200, 25);
        }
      }
    },
    mounted(){
      // var arrSc=this.dataSc;
      // console.log(arrSc);
      this.drawCanvas();
      window.onload=function(){
        
    };
    },
    watch:{
    dataSc:{
        handler(newValue){
          // console.log(this.dataBu);
          // console.log(this.dataSc);
        this.drawCanvas();
        },
        deep:true
    }
}
}



</script>

