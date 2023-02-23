<template>
	<div style="width: 480px; height:393px;">
		<h3 style="text-align: left;">2022年全国各省政府响应指数和确诊人数的关系</h3>
		<div id="scatterPlot" style="width: 100%; height: 300px;">
			<!-- 定义散点图的容器 -->
			<div id="scatter-plot-container"></div>
		</div>
		<div style="width: 100%; text-align: center; color: white;">
			<span style="display: inline-block; width: 10px; height: 10px; background-color: blue;"></span>
			政府响应指数
			<span
				style="display: inline-block; width: 10px; height: 10px; background-color: red; margin-left: 20px;"></span>
			确诊人数
		</div>
	</div>
</template>


<style>
	text {
		fill: white
	}
	/* 定义散点图容器的样式 */
	#scatter-plot-container {
		margin: 0 auto;
	}
	h3 {
		margin-left: 5px;
		font-size:10px;
		color: #ffffff;
	}

	/* 定义坐标轴的样式 */
	.axis {
		font-size: 12px;
	}

	/* 定义坐标轴的线条样式 */
	.axis path,
	.axis line {
		fill: none;
		/* 填充 */
		stroke: #ffffff;
		/* 线条颜色 */
		shape-rendering: crispEdges;
		/* 边缘修整 */
	}

	.axis line {
		stroke: #ddd;
		stroke-dasharray: 2 2;
	}

	/* 定义散点的样式 */
	.dot {
		fill: blue;
		/* 填充颜色 */
		stroke-width: 1.5px;
		/* 线条宽度 */
	}

	/* 定义x轴的样式 */
	.xAxis path,
	.xAxis line {
		fill: none;
		/* 填充 */
		stroke: #fff;
		/* 线条颜色 */
		shape-rendering: crispEdges;
		/* 边缘修整 */
	}

	/* 定义y轴的样式 */
	.yAxis path,
	.yAxis line {
		fill: none;
		/* 填充*/
		stroke: #fff;
		/* 线条颜色 */
		shape-rendering: crispEdges;
		/* 边缘修整 */
	}
</style>
<script>
	window.addEventListener('load',function(){
		

	// 定义数据
	var data = [{
			x: 0,
			y: 0
		},
		{
			x: 5,
			y: 500
		},
		{
			x: 10,
			y: 1000
		},
		{
			x: 15,
			y: 1500
		},
		{
			x: 20,
			y: 2000
		},
		{
			x: 25,
			y: 2500
		},
		{
			x: 30,
			y: 3000
		},
		{
			x: 35,
			y: 3500
		},
		{
			x: 40,
			y: 4000
		},
	];





	// 添加100个散点数据
	for (var i = 0; i < 100; i++) {
		data.push({
			x: Math.floor(Math.random() * 40),
			y: Math.floor(Math.random() * 4000),
		});
	}
	// 定义散点样式
	var scatterStyle = {

	};


	// 定义图表配置项
	var option = {
		xAxis: {
			type: "value",
		},
		yAxis: {
			type: "value",
		},
		series: [{
			type: "scatter",
			data: data,
			itemStyle: scatterStyle,
			// animation: true, // 开启动画效果
		}, ],
	};


	// 定义图表边距
	var margin = {
			top: 20,
			right: 20,
			bottom: 30,
			left: 40
		},
		width = 350 - margin.left - margin.right,
		height = 250 - margin.top - margin.bottom;
	// 定义x轴比例尺
	var x = d3.scaleLinear().range([0, width]);
	var ctx;
	// 定义y轴比例尺
	var y = d3.scaleLinear().range([height, 0]);

	// 定义x轴坐标轴
	var xAxis = d3.axisBottom(x)
		.ticks(9)
		.tickSizeInner(-height)
		.tickSizeOuter(0)
		.tickPadding(10);

	// 定义y轴坐标轴
	var yAxis = d3.axisLeft(y)
		.ticks(9)
		.tickSizeInner(-width)
		.tickSizeOuter(0)
		.tickPadding(10);

	// 创建SVG元素
	var svg = d3
		.select("#scatter-plot-container")
		.append("svg")
		.attr("width", width + margin.left + margin.right)
		.attr("height", height + margin.top + margin.bottom)
		.append("g")
		.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

	// 定义x、y轴的数据范围
	x.domain(d3.extent(data, function(d) {
		return d.y;
	})).nice();
	y.domain(d3.extent(data, function(d) {
		return d.x;
	})).nice();

	// 添加x轴
	svg.append("g")
		.attr("class", "xAxis")
		.call(xAxis)
		.attr("transform", "translate(0," + height + ")");

	// 添加y轴
	svg.append("g")
		.attr("class", "yAxis")
		.call(yAxis);

	svg.selectAll(".dot")
		.data(data)
		.enter().append("circle")
		.attr("class", "dot")
		.attr("r", 5)
		.attr("cx", function(d) {
			return x(d.y);
		})
		.attr("cy", function(d) {
			return y(d.x);
		})
		.style("fill", function(d) {
			return d.x > 20 ? "red" : "blue";
		});

	// 添加散点数据的函数

	// function addData() {
		// 随机生成新的散点数据
		var newData = [];
		for (var i = 0; i < 5; i++) {
			var x = Math.floor(Math.random() * 41);
			var y = Math.floor(Math.random() * 4001);
			newData.push({
				x: x,
				y: y
			});
		}

		// 将新的散点数据添加到原始数据中
		data = data.concat(newData);
		// 绘制散点图
		// if(canvas!=null){

		// }
		// ctx.clearRect(0, 0, canvas.width, canvas.height);
		for (var i = 0; i < data.length; i++) {
			var point = data[i];
			var color = colors[i % 2];
			ctx.beginPath();
			ctx.arc(point.x * 10, canvas.height - point.y / 4, 5, 0, Math.PI * 2);
			ctx.fillStyle = color;
			ctx.fill();
			ctx.closePath();
		}
	// }
	// // 每隔24小时执行一次添加散点数据的操作
	// setInterval(addData, 24 * 60 * 60 * 1000);
		})
</script>
