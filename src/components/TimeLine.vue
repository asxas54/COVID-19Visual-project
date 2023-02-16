<template>
  <canvas id="title" width="360" height="80"></canvas>
  <canvas id="school" width="360" height="80"></canvas>
  <canvas id="work" width="360" height="80"></canvas>
</template>

<style>
canvas {
  /*border: 1px solid black;*/
  margin: 0 auto;
  display: block;
  /*background-color: #AAAAAA;*/
}
</style>

<script>
window.onload=function(){
  var canvas_title = document.getElementById("title");
  var ctx_title = canvas_title.getContext("2d");

  var getPixelRatio = function (context){
      var devicePixelRatio = window.devicePixelRatio || 1;
      var backingStoreRatio = ctx_title.webkitBackingStorePixelRatio ||
          ctx_title.mozBackingStorePixelRatio ||
          ctx_title.msBackingStorePixelRatio ||
          ctx_title.oBackingStorePixelRatio ||
          ctx_title.backingStorePixelRatio || 1;
      return (devicePixelRatio / backingStoreRatio);
  };

  var radio = getPixelRatio(ctx_title);
  // console.log(radio);
  canvas_title.style.width = canvas_title.width + 'px';
  canvas_title.style.height = canvas_title.height + 'px';
  canvas_title.width = canvas_title.width * radio;
  canvas_title.height = canvas_title.height * radio;

// 画标题
  ctx_title.font = "bold 18px Arial";
  ctx_title.textAlign = "center";
  ctx_title.fillStyle = "white";
  ctx_title.fillText("北京市2022年政策追踪", canvas_title.width / 2, 24);

// 颜色提示
  ctx_title.beginPath();
  ctx_title.moveTo(0, 55);
  ctx_title.lineTo(30, 55);
  ctx_title.strokeStyle = "green";
  ctx_title.lineWidth = 4;
  ctx_title.font = "14px Arial";
  ctx_title.fillText("等级0，无措施", 80, 60);
  ctx_title.stroke();

  ctx_title.beginPath();
  ctx_title.moveTo(0, 55+20);
  ctx_title.lineTo(30, 55+20);
  ctx_title.strokeStyle = "yellow";
  ctx_title.lineWidth = 4;
  ctx_title.font = "14px Arial";
  ctx_title.fillText("等级1，建议措施", 87, 60+20);
  ctx_title.stroke();

  ctx_title.beginPath();
  ctx_title.moveTo(canvas_title.width-145, 55);
  ctx_title.lineTo(canvas_title.width-115, 55);
  ctx_title.strokeStyle = "red";
  ctx_title.lineWidth = 4;
  ctx_title.font = "14px Arial";
  ctx_title.fillText("等级2，部分强制", canvas_title.width-60, 60);
  ctx_title.stroke();

  ctx_title.beginPath();
  ctx_title.moveTo(canvas_title.width-145, 55+20);
  ctx_title.lineTo(canvas_title.width-115, 55+20);
  ctx_title.strokeStyle = "black";
  ctx_title.lineWidth = 4;
  ctx_title.font = "14px Arial";
  ctx_title.fillText("等级3，完全强制", canvas_title.width-60, 60+20);
  ctx_title.stroke();

  const canvas_school = document.getElementById("school");
  const ctx_school = canvas_school.getContext("2d");

  canvas_school.style.width = canvas_school.width + 'px';
  canvas_school.style.height = canvas_school.height + 'px';
  canvas_school.width = canvas_school.width * radio;
  canvas_school.height = canvas_school.height * radio;

  // 直线含义
  ctx_school.font = "16px Arial";
  ctx_school.fillStyle = "white";
  ctx_school.fillText("学校关闭政策", 0, 55);

  // 画直线1.1
  ctx_school.beginPath();
  ctx_school.moveTo(110, 50);
  ctx_school.lineTo((canvas_school.width-110)/4+110, 50);
  ctx_school.strokeStyle = "yellow";
  ctx_school.lineWidth = 3;
  ctx_school.stroke();

  // 画直线1.2
  ctx_school.beginPath();
  ctx_school.moveTo((canvas_school.width-110)/4+110, 50);
  ctx_school.lineTo(canvas_school.width, 50);
  ctx_school.strokeStyle = "red";
  ctx_school.stroke();

  // 画空心小圆-20220101
  // 为了擦除空心小圆里面的直线，需要先把空心小圆包含的区域涂成背景色（一般是白色），再重新画一个空心小圆
  ctx_school.beginPath();
  ctx_school.arc(110, 50, 6, 0, 2 * Math.PI, false);
  ctx_school.fillStyle = "black";
  ctx_school.fill();

  ctx_school.beginPath();
  ctx_school.arc(110, 50, 4, 0, 2 * Math.PI, false);
  ctx_school.lineWidth = 1;
  ctx_school.strokeStyle = "white";
  ctx_school.stroke();

  // 画空心小圆-20220314大概四分之一
  ctx_school.beginPath();
  ctx_school.arc((canvas_school.width-110)/4+110, 50, 6, 0, 2 * Math.PI, false);
  ctx_school.fill();

  ctx_school.beginPath();
  ctx_school.arc((canvas_school.width-110)/4+110, 50, 4, 0, 2 * Math.PI, false);
  ctx_school.stroke();

  // 画竖直的16px的直线
  ctx_school.beginPath();
  ctx_school.moveTo(110, 50+4);
  ctx_school.lineTo(110, 50+20);
  ctx_school.stroke();

  ctx_school.beginPath();
  ctx_school.moveTo((canvas_school.width-110)/4+110, 50-4);
  ctx_school.lineTo((canvas_school.width-110)/4+110, 50-20);
  ctx_school.stroke();

  //政策调整
  ctx_school.font = "12px Arial";
  ctx_school.fillStyle = "white";
  ctx_school.fillText("1月1日学校建议关闭", 60, 80);

  ctx_school.fillText("3月14日某些级别学校要求关闭", (canvas_school.width-110)/4+40, 25);

  const canvas_work = document.getElementById("work");
  const ctx_work = canvas_work.getContext("2d");

  canvas_work.style.width = canvas_work.width + 'px';
  canvas_work.style.height = canvas_work.height + 'px';
  canvas_work.width = canvas_work.width * radio;
  canvas_work.height = canvas_work.height * radio;

  // 直线含义
  ctx_work.font = "16px Arial";
  ctx_work.fillStyle = "white";
  ctx_work.fillText("办公场所政策", 0, 55);

  // 画直线1.1
  ctx_work.beginPath();
  ctx_work.moveTo(110, 50);
  // ctx_work.lineTo((canvas_work.width-110)/4+110, 50);
  ctx_work.lineTo(canvas_work.width-(canvas_work.width-110)/12, 50);
  ctx_work.strokeStyle = "red";
  ctx_work.lineWidth = 3;
  ctx_work.stroke();

  // 画直线1.2
  ctx_work.beginPath();
  ctx_work.moveTo(canvas_work.width-(canvas_work.width-110)/12, 50);
  ctx_work.lineTo(canvas_work.width, 50);
  ctx_work.strokeStyle = "yellow";
  ctx_work.stroke();

  // 画空心小圆-20220101
  // 为了擦除空心小圆里面的直线，需要先把空心小圆包含的区域涂成背景色（一般是白色），再重新画一个空心小圆
  ctx_work.beginPath();
  ctx_work.arc(110, 50, 6, 0, 2 * Math.PI, false);
  ctx_work.fillStyle = "black";
  ctx_work.fill();

  ctx_work.beginPath();
  ctx_work.arc(110, 50, 4, 0, 2 * Math.PI, false);
  ctx_work.lineWidth = 1;
  ctx_work.strokeStyle = "white";
  ctx_work.stroke();

  // 画空心小圆-20220314大概四分之一
  ctx_work.beginPath();
  ctx_work.arc(canvas_work.width-(canvas_work.width-110)/12, 50, 6, 0, 2 * Math.PI, false);
  ctx_work.fill();

  ctx_work.beginPath();
  ctx_work.arc(canvas_work.width-(canvas_work.width-110)/12, 50, 4, 0, 2 * Math.PI, false);
  ctx_work.stroke();

  // 画竖直的16px的直线
  ctx_work.beginPath();
  ctx_work.moveTo(110, 50+4);
  ctx_work.lineTo(110, 50+20);
  ctx_work.stroke();

  ctx_work.beginPath();
  ctx_work.moveTo(canvas_work.width-(canvas_work.width-110)/12, 50-4);
  ctx_work.lineTo(canvas_work.width-(canvas_work.width-110)/12, 50-20);
  ctx_work.stroke();

  //政策调整
  ctx_work.font = "12px Arial";
  ctx_work.fillStyle = "white";
  ctx_work.fillText("1月1日部分岗位在家办公", 60, 80);

  ctx_work.fillText("12月22日建议在家办公", canvas_work.width-(canvas_work.width-110)/12-100, 25);
};


</script>

