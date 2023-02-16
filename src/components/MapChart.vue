<template>
   <div class="maptitle"><input type="date" name="" id="inputDate" value="2020-01-01">各省<span
				id="titleText">严格指数</span></div>
		<canvas id="mapchar" width="600" height="315"></canvas>
		<div class="legend">
			<div class="legend1"></div><span class="legenddata">1~25</span><br />
			<div class="legend2"></div><span class="legenddata">25~50</span><br />
			<div class="legend3"></div><span class="legenddata">50~75</span><br />
			<div class="legend4"></div><span class="legenddata">75以上</span><br />
			<div class="legend5"></div><span class="legenddata">无数据</span><br />
		</div>
		<div class="radio">
			<label><input type="radio" checked name="target" id="target"
					value="StringencyIndex_Average" />严格指数</label><br />
			<label><input type="radio" name="target" id="target"
					value="GovernmentResponseIndex_Average" />政府响应指数</label><br />
			<label><input type="radio" name="target" id="target"
					value="ContainmentHealthIndex_Average" />遏制和健康指数</label><br />
			<label><input type="radio" name="target" id="target" value="EconomicSupportIndex" />经济支持指数</label><br />
		</div>

</template>

<script>
window.addEventListener('load', function() {
	mapchar("mapchar");

	function mapchar(mapchar) {
		var canvas = document.getElementById(mapchar);
		var canvasW = canvas.width
		var canvasH = canvas.height
		var geoCenterX = 0,
			geoCenterY = 0 // 地图区域的经纬度中心点
		var geoData = []; //绘制地图需要的数据
		var ctx = canvas.getContext('2d')
		var selectvalue = "StringencyIndex_Average"; //选择值
		//==========模糊处理===================
		canvas.width = 640 * 2;
		canvas.height = 500 * 2;
		canvas.style.width = 640 + "px";
		canvas.style.height = 500 + "px";
		ctx.scale(2, 2);
		var scale;
		var jsonData = []; //csv文件数据
		var titleText = "严格指数"; //标题,默认为严格指数
		var dateNum = "20200101" //日期数字,默认为20200101
		//==================解析csv文件=======================
		$.ajax({
			type: 'GET',
			url: "src/data/OxCGRT_CHN_latest - 2-6.csv",
			dataType: 'text',
			// async: false,
			success: function(data) {
				jsonData = $.csv.toObjects(data);
			},
			error: function(e) {
				alert('An error occurred while processing API calls');
				console.log("API call Failed: ", e);
			},
		});
		//保证在绘制地图之前拿到csv文件数据
		init()

		function init() {
			var request = new XMLHttpRequest()
			request.open('get', 'https://geojson.cn/api/data/china.json')
			//===============send()=============
			//发送 HTTP 请求。如果是异步请求（默认为异步请求）
			//则此方法会在请求发送后立即返回；如果是同步请求
			//则此方法直到响应到达后才会返回。
			request.send()
			request.onload = function() {
				//==============request.status ==================
				//只读属性 XMLHttpRequest.status 返回了 XMLHttpRequest 响应中的数字状态码。
				//status 的值是一个无符号短整型。
				//在请求完成前，status 的值为 0。
				//值得注意的是，如果 XMLHttpRequest 出错
				//浏览器返回的 status 也为 0。
				if (request.status === 200) {
					geoData = JSON.parse(request.responseText)
					getBoxArea()
					drawMap()
				}
			}
		}
		//======根据省份中文转换为英文=================
		function getEnglish(country) {
			var countryE = "";
			switch (country) {
				case "安徽":
					countryE = "Anhui"
					break;
				case "北京":
					countryE = "Beijing"
					break;
				case "重庆":
					countryE = "Chongqing"
					break;
				case "福建":
					countryE = "Fujian"
					break;
				case "广东":
					countryE = "Guangdong"
					break;
				case "甘肃":
					countryE = "Gansu"
					break;
				case "广西":
					countryE = "Guangxi"
					break;
				case "贵州":
					countryE = "Guizhou"
					break;
				case "河南":
					countryE = "Henan"
					break;
				case "湖北":
					countryE = "Hubei"
					break;
				case "河北":
					countryE = "Hebei"
					break;
				case "海南":
					countryE = "Hainan"
					break;
				case "黑龙江":
					countryE = "Heilongjiang"
					break;
				case "湖南":
					countryE = "Hunan"
					break;
				case "吉林":
					countryE = "Jilin"
					break;
				case "江苏":
					countryE = "Jiangsu"
					break;
				case "江西":
					countryE = "Jiangxi"
					break;
				case "辽宁":
					countryE = "Liaoning"
					break;
				case "内蒙古":
					countryE = "Inner Mongolia"
					break;
				case "宁夏":
					countryE = "Ningxia"
					break;
				case "青海":
					countryE = "Qinghai"
					break;
				case "四川":
					countryE = "Sichuan"
					break;
				case "山东":
					countryE = "Shandong"
					break;
				case "上海":
					countryE = "Shanghai"
					break;
				case "陕西":
					countryE = "Shaanxi"
					break;
				case "山西":
					countryE = "Shanxi"
					break;
				case "天津":
					countryE = "Tianjin"
					break;
				case "新疆":
					countryE = "Xinjiang"
					break;
				case "西藏":
					countryE = "Tibet"
					break;
				case "浙江":
					countryE = "Zhejiang"
					break;
				case "云南":
					countryE = "Yunnan"
					break;
			}
			return countryE;
		}
		// 分三步，清空画布、绘制地图各子区域、标注城市名称
		function drawMap() {
			//清空画布
			ctx.clearRect(0, 0, canvasW, canvasH)
			// 画布背景
			ctx.fillStyle = '#393c49'
			ctx.fillRect(0, 0, canvasW, canvasH)
			drawArea() // 绘制地图各子区域
			drawText()
		}
		// 绘制地图各子区域
		function drawArea() {
			var dataArr = geoData.features
			var cursorFlag = false
			for (var i = 0; i < dataArr.length; i++) {
				var centerX = canvasW / 2
				var centerY = canvasH / 2
				// dataArr[i].properties.color='#00cccc'
				// console.log(dataArr[i].properties.color);
				ctx.fillStyle = '#7bb8b8'

				for (var a = 0; a < jsonData.length; a++) {
					if (getEnglish(dataArr[i].properties.name) == jsonData[a].RegionName && jsonData[a]
						.Date == dateNum) {
						//将选择的数据映射成相应的颜色,以对象方式添加到jsonData
						if (jsonData[a][selectvalue] == 0) {
							dataArr[i].properties.color = '#7bb8b8'
						} else if (jsonData[a][selectvalue] > 0 && jsonData[a][selectvalue] < 25) {
							dataArr[i].properties.color = '#7efac6'
						} else if (jsonData[a][selectvalue] >= 25 && jsonData[a][selectvalue] < 50) {
							dataArr[i].properties.color = '#3fc190'
						} else if (jsonData[a][selectvalue] >= 50 && jsonData[a][selectvalue] < 75) {
							dataArr[i].properties.color = '#018a5e'
						} else {
							dataArr[i].properties.color = '#003d1b'
						}
					}
				}

				dataArr[i].geometry.coordinates.forEach(area => {
					ctx.save()
					ctx.beginPath()
					ctx.translate(centerX, centerY)
					area[0].forEach((elem, index) => {
						let position = toScreenPosition(elem[0], elem[1])
						if (index === 0) {
							ctx.moveTo(position.x, position.y)
						} else {
							ctx.lineTo(position.x, position.y)
						}
					})
					ctx.closePath()
					ctx.strokeStyle = '#00cccc'
					ctx.lineWidth = 1
					ctx.fillStyle = dataArr[i].properties.color;
					ctx.fill()
					ctx.stroke()
					ctx.restore()
				});
			}
		}
		//当单选框被点击时,根据所选不同选项呈现不同的数据
		var radios = document.querySelectorAll("#target");
		var titleText = document.querySelector("#titleText");
		for (var q = 0; q < radios.length; q++) {
			radios[q].addEventListener("click", setvalue)

			function setvalue() {
				selectvalue = this.value;
				var title = ""
				//当单选框的所选值被修改时,动态改变标题栏的标题内容
				switch (selectvalue) {
					case "StringencyIndex_Average":
						title = "严格指数";
						break;
					case "GovernmentResponseIndex_Average":
						title = "政府响应指数";
						break;
					case "ContainmentHealthIndex_Average":
						title = "遏制和健康指数";
						break;
					case "EconomicSupportIndex":
						title = "经济支持指数";
						break;
				}
				titleText.innerHTML = title;
				init()
				drawMap()
			}
		}
		var date = document.querySelector("#inputDate");
		date.addEventListener("change", function() {
			dateNum = date.value.replace(/\-/g, "")
			init()
			drawMap()
		})
		// 标注地图上的城市名称
		function drawText() {
			var centerX = canvasW / 2
			var centerY = canvasH / 2
			geoData.features.forEach(item => {
				ctx.save()
				ctx.beginPath()
				ctx.translate(centerX, centerY) // 将画笔移至画布的中心
				ctx.fillStyle = '#fff'
				ctx.font = '12px Microsoft YaHei'
				ctx.textAlign = 'center' //字体居中
				ctx.textBaseLine = 'center' //基线居中
				var x = 0,
					y = 0
				//  因不同的geojson文件中中心点属性信息不同，这里需要做兼容性处理
				if (item.properties.cp) {
					x = item.properties.cp[0]
					y = item.properties.cp[1]
				} else if (item.properties.centroid) {
					x = item.properties.centroid[0]
					y = item.properties.centroid[1]
				} else if (item.properties.center) {
					x = item.properties.center[0]
					y = item.properties.center[1]
				}
				var position = toScreenPosition(x, y)
				ctx.fillText(item.properties.name, position.x, position.y);
				ctx.restore()
			})
		}
		// 将经纬度坐标转换为屏幕坐标
		function toScreenPosition(horizontal, vertical) {
			return {
				x: (horizontal - geoCenterX) * scale,
				y: (geoCenterY - vertical) * scale
			}
		}
		// 获取包围盒范围，计算包围盒中心经纬度坐标，计算地图缩放系数
		function getBoxArea() {
			var N = -90,
				S = 90,
				W = 180,
				E = -180
			geoData.features.forEach(item => {
				// 将MultiPolygon和Polygon格式的地图处理成统一数据格式
				if (item.geometry.type === 'Polygon') {
					item.geometry.coordinates = [item.geometry.coordinates]
				}
				// 取四个方向的极值
				item.geometry.coordinates.forEach(area => {
					let areaN = -90,
						areaS = 90,
						areaW = 180,
						areaE = -180
					area[0].forEach(elem => {
						if (elem[0] < W) {
							W = elem[0]
						}
						if (elem[0] > E) {
							E = elem[0]
						}
						if (elem[1] > N) {
							N = elem[1]
						}
						if (elem[1] < S) {
							S = elem[1]
						}
					})
				})
			})
			// 计算包围盒的宽高
			var width = Math.abs(E - W)
			var height = Math.abs(N - S)
			var wScale = canvasW / width
			var hScale = canvasH / height
			// 计算地图缩放系数
			scale = wScale > hScale ? hScale : wScale
			// 获取包围盒中心经纬度坐标
			geoCenterX = (E + W) / 2
			geoCenterY = (N + S) / 2
		}
	}
})

</script>

<style>
   	.mapchar {
				display: inline-block;
				width: 640px;
				height: 500px;
				position: relative;
			}

			.legend {
				width: 110px;
				height: 105px;
				position: absolute;
                top: 200px;
                left: 3px;
			}

			.legend>.legend1,
			.legend2,
			.legend3,
			.legend4,
			.legend5 {
				width: 20px;
				height: 10px;
				display: inline-block;
			}

			.legenddata {
				margin-left: 5px;
				font-size: 12px;
				color: white;
			}

			.legend>.legend1 {
				background-color: #7efac6;
			}

			.legend>.legend2 {
				background-color: #3fc190;
			}

			.legend>.legend3 {
				background-color: #018a5e;
			}

			.legend>.legend4 {
				background-color: #003d1b;
			}

			.legend>.legend5 {
				background-color: #7bb8b8;
			}

			.radio {
				font-size: 12px;
				color: white;
				font-family: "微软雅黑";
				position: absolute;
                line-height: 18px;
                left: 69px;
                top: 225px;
			}

			.maptitle {
				width: 600px;
				height: 20px;
				line-height: 20px;
				text-align: center;
				font-size: 15px;
				position: absolute;
				left: 10px;
				top: 10px;
				color: white;
			}

			.maptitle>input {
				font-family: "微软雅黑";
				border: none;
				color: white;
				background-color: #393c49;
                width: 100px;
			}
</style>