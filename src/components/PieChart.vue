<template>
    <!-- <div> -->
        <div id="container">
            <div id="content1"><div id="canvas1"></div></div>
            <div id="content2" style="display: none" ><div id="canvas2"></div></div>
            <div id="content3" style="display: none"><div id="canvas3"></div></div>
        </div>
        <ul id="tab">
            <li id="tab1" value="1"></li>
            <li id="tab2" value="2"></li>
            <li id="tab3" value="3"></li>
        </ul>
        <!-- <h style="font-size: 0px;">饼图</h> -->
    <!-- </div> -->
    </template>
    
    
    <style>

            /* #tab li {
                float: left;
                list-style: none;
                width: 20px;
                height: 20px;
                line-height: 20px;
                cursor: pointer;
                text-align: center;
            } */
            #container {
                width: 100%;
                height: 100%;
                /* position: relative; */
                padding-top: 25px;
            }
            #content1, #content2, #content3 {
                width: 100%;
                height: 100%;
               /* width: 300px;
                height: 100px;
                padding: 30px; */
                /* position: absolute; */
    /*            top: 40px;*/
            /* padding-top: 20px; */
                left: 0;
            }
            #tab {
                height: 20px;
                text-align: center;
                padding-left: 180px;
                position: absolute;
                top: 360px;
            }
            #tab li {
                display: inline-block;
                margin-right: 10px;
                width: 8px!important;
                height: 8px!important;
                border-radius: 50%!important;
                border: 2px solid gray;
            }
            #tab li:hover {
                cursor: pointer;
                border: 2px solid #fff;
                background-color:#fff;
            }
            #tab li.active {
                border: 2px solid gray;
                background-color: blue;
            }
    </style>
    
    <script>
    window.addEventListener('load', function() {
        function roundRect(ctx, x, y, width, height, radius) {
        ctx.beginPath();
        if (width > 0) {
            ctx.moveTo(x + radius, y);
        } else {
            ctx.moveTo(x - radius, y);
        }
    
        ctx.arcTo(x + width, y, x + width, y + height, radius);
        ctx.arcTo(x + width, y + height, x, y + height, radius);
        ctx.arcTo(x, y + height, x, y, radius);
    
        if (width > 0) {
            ctx.arcTo(x, y, x + radius, y, radius);
        } else {
            ctx.arcTo(x, y, x - radius, y, radius);
        }
    }
    /**
     * 图表基类
     */
     class Chart {
        constructor(container) {
            this.container = container;
            
            this.canvas = document.createElement('canvas');
            this.ctx = this.canvas.getContext('2d');
            this.W = 540;
            this.H = 460;
            this.padding = 5;
            this.paddingTop = 90;
            this.title = '';
            this.legend = [];
            this.series = [];
            this.xAxis = {};
            this.yAxis = [];
            this.animateArr = [];
            this.info = {};
            this.drawing = false;
        }
        init(opt) {
            Object.assign(this, opt);
            if (!this.container) return;
            //通过缩小一倍，解决字体模糊问题
            this.W *= 2;
            this.H *= 2.1;
            this.canvas.width = this.W;
            this.canvas.height = this.H;
            this.canvas.style.width = this.W / 2.5 + 'px';
            this.canvas.style.height = this.H / 2.5 + 'px';
            this.container.style.position = 'relative';
            this.tip = document.createElement('div');
            this.tip.style.cssText = 'display: none; position: absolute; opacity: 0.5; background: #000; color: #fff; border-radius: 5px; padding: 5px; font-size: 8px; z-index: 99;';
            this.container.appendChild(this.canvas);
            this.container.appendChild(this.tip);
            this.create();
            this.bindEvent();
        }
        showInfo(pos, title, arr) {
            var box = this.canvas.getBoundingClientRect(),
                con = this.container.getBoundingClientRect(),
                html = '',
                txt = '';
            html += '<p>' + title + '</p>';
            arr.forEach(obj => {
                txt = this.yAxis.formatter ? this.yAxis.formatter.replace('{value}', obj.num) : obj.num;
                html += '<p>' + obj.name + ': ' + txt + '</p>';
            })
            this.tip.innerHTML = html;
            this.tip.style.left = (pos.x + (box.left - con.left) + 15) + 'px';
            this.tip.style.top = (pos.y + (box.top - con.top) + 15) + 'px';
            this.tip.style.display = 'block';
        }
    }
    
    
    /**
     * 饼图
     */
    class Pie extends Chart {
        constructor(container) {
            super(container);
        }
        bindEvent() {
            var that = this,
                canvas = that.canvas,
                ctx = that.ctx;
            if (!this.data.length) return;
            this.canvas.addEventListener('mousemove', function(e) {
                var isLegend = false;
                var box = canvas.getBoundingClientRect(),
                    pos = {
                        x: e.clientX - box.left,
                        y: e.clientY - box.top
                    };
                // 标签
                for (var i = 0, item, len = that.legend.length; i < len; i++) {
                    item = that.legend[i];
                    roundRect(ctx, item.x, item.y, item.w, item.h, item.r);
                    if (ctx.isPointInPath(pos.x * 2, pos.y * 2)) {
                        canvas.style.cursor = 'pointer';
                        if (!item.hide) {
                            that.clearGrid(i);
                        }
                        isLegend = true;
                        break;
                    }
                    canvas.style.cursor = 'default';
                    that.tip.style.display = 'none';
                }
    
                if (isLegend) return;
                // 图表
                var startAng = -Math.PI / 2;
                for (var i = 0, l = that.animateArr.length; i < l; i++) {
                    item = that.animateArr[i];
                    if (item.hide) continue;
                    ctx.beginPath();
                    ctx.moveTo(that.W / 2, that.H / 2);
                    ctx.arc(that.W / 2, that.H / 2, that.H / 3, startAng, startAng + item.ang, false);
                    ctx.closePath();
                    startAng += item.ang;
                    if (ctx.isPointInPath(pos.x * 2, pos.y * 2)) {
                        canvas.style.cursor = 'pointer';
                        that.clearGrid(i);
                        that.showInfo(pos, that.toolTip, [{ name: item.name, num: item.num + ' (' + item.percent + '%)' }]);
                        break;
                    }
                    canvas.style.cursor = 'default';
                    that.clearGrid();
                }
    
            }, false);
            this.canvas.addEventListener('mousedown', function(e) {
                e.preventDefault();
                var box = that.canvas.getBoundingClientRect();
                var pos = {
                    x: e.clientX - box.left,
                    y: e.clientY - box.top
                };
                for (var i = 0, item, len = that.legend.length; i < len; i++) {
                    item = that.legend[i];
                    roundRect(ctx, item.x, item.y, item.w, item.h, item.r);
                    if (ctx.isPointInPath(pos.x * 2, pos.y * 2)) {
                        that.data[i].hide = !that.data[i].hide;
                        that.create();
                        break;
                    }
                }
            }, false);
    
        }
        clearGrid(index) {
            var that = this,
                ctx = that.ctx,
                canvas = that.canvas,
                item, startAng = -Math.PI / 2,
                len = that.animateArr.filter(item => !item.hide).length,
                j = 0,
                angle = 0,
                r = that.H / 3;
            ctx.clearRect(0, 0, that.W, that.H);
            that.draw();
            ctx.save();
            ctx.translate(that.W / 2, that.H / 2);
    
            for (var i = 0, l = that.animateArr.length; i < l; i++) {
                item = that.animateArr[i];
                if (item.hide) continue;
                ctx.strokeStyle = item.color;
                ctx.fillStyle = item.color;
                angle = j >= len - 1 ? Math.PI * 2 - Math.PI / 2 : startAng + item.ang;
                ctx.beginPath();
                ctx.moveTo(0, 0);
                if (index === i) {
                    ctx.save();
                    // ctx.shadowColor='hsla(0,0%,50%,1)';
                    ctx.shadowColor = item.color;
                    ctx.shadowBlur = 5;
                    ctx.arc(0, 0, r + 20, startAng, angle, false);
                    ctx.closePath();
                    ctx.fill();
                    ctx.stroke();
                    ctx.restore();
                } else {
                    ctx.arc(0, 0, r, startAng, angle, false);
                    ctx.closePath();
                    ctx.fill();
                }
                //画描述
                var tr = r + 40,
                    tw = 0,
                    tAng = startAng + item.ang / 2,
                    x = tr * Math.cos(tAng),
                    y = tr * Math.sin(tAng);
    
                ctx.lineWidth = 2;
                ctx.lineCap = 'round';
                ctx.beginPath();
                ctx.moveTo(0, 0);
                ctx.lineTo(x, y);
                if (tAng >= -Math.PI / 2 && tAng <= Math.PI / 2) {
                    ctx.lineTo(x + 30, y);
                    ctx.fillText(item.num, x + 40, y + 10);
                } else {
                    tw = ctx.measureText(item.num).width; //计算字符长度
                    ctx.lineTo(x - 30, y);
                    ctx.fillText(item.num, x - 40 - tw, y + 10);
                }
    
                ctx.stroke();
                startAng += item.ang;
                j++;
            }
            ctx.restore();
        }
        animate() {
            var that = this,
                ctx = that.ctx,
                canvas = that.canvas,
                item, startAng, ang,
                isStop = true;
    
            (function run() {
                isStop = true;
                ctx.save();
                ctx.translate(that.W / 2, that.H / 2);
                ctx.fillStyle = '#fff';
                ctx.beginPath();
                ctx.arc(0, 0, that.H / 3 + 30, 0, Math.PI * 2, false);
                ctx.fill();
                for (var i = 0, l = that.animateArr.length; i < l; i++) {
                    item = that.animateArr[i];
                    if (item.hide) continue;
                    startAng = -Math.PI / 2;
                    that.animateArr.forEach((obj, j) => {
                        if (j < i && !obj.hide) { startAng += obj.cur; }
                    });
    
                    ctx.fillStyle = item.color;
                    if (item.last > item.ang) {
                        ang = item.cur - 0.05;
                        if (ang < item.ang) {
                            item.cur = item.last = item.ang;
                        }
                    } else {
                        ang = item.cur + 0.05;
                        if (ang > item.ang) {
                            item.cur = item.last = item.ang;
                        }
                    }
                    if (item.cur != item.ang) {
                        item.cur = ang;
                        isStop = false;
                    }
    
                    ctx.beginPath();
                    ctx.moveTo(0, 0);
                    ctx.arc(0, 0, that.H / 3, startAng, startAng + item.cur, false);
                    ctx.closePath();
                    ctx.fill();
                }
                ctx.restore();
                if (isStop) {
                    that.clearGrid();
                    return;
                }
                requestAnimationFrame(run);
            }());
        }
        create() {
            this.initData();
            this.draw();
            this.animate();
        }
        initData() {
            var that = this,
                item,
                total = 0;
            if (!this.data || !this.data.length) {
                return; }
            this.legend.length = 0;
            for (var i = 0; i < this.data.length; i++) {
                item = this.data[i];
                // 赋予没有颜色的项
                if (!item.color) {
                    var hsl = i % 2 ? 180 + 30 * (i - 1) : 30 * i / 2;
                    item.color = 'hsla(' + hsl + ',70%,60%,1)';
                }
                item.name = item.name || 'unnamed';
    
                this.legend.push({
                    hide: !!item.hide,
                    name: item.name,
                    color: item.color,
                    x: 50,
                    y: that.paddingTop + 20 + i * 40,
                    w: 50,
                    h: 20,
                    r: 5
                });
    
                if (item.hide) continue;
                total += item.value;
            }
    
            for (var i = 0; i < this.data.length; i++) {
                item = this.data[i];
                if (!this.animateArr[i]) {
                    this.animateArr.push({
                        i: i,
                        create: true,
                        hide: !!item.hide,
                        name: item.name,
                        color: item.color,
                        num: item.value,
                        percent: Math.round(item.value / total * 10000) / 100,
                        ang: Math.round(item.value / total * Math.PI * 2 * 100) / 100,
                        last: 0,
                        cur: 0
                    });
                } else { //更新               
                    if (that.animateArr[i].hide && !item.hide) {
                        that.animateArr[i].create = true;
                        that.animateArr[i].cur = 0;
                        that.animateArr[i].last = 0;
                    } else {
                        that.animateArr[i].create = false;
                    }
                    that.animateArr[i].hide = item.hide;
                    that.animateArr[i].percent = Math.round(item.value / total * 10000) / 100;
                    that.animateArr[i].ang = Math.round(item.value / total * Math.PI * 2 * 100) / 100;
                }
            }
        }
        draw() {
            var item, ctx = this.ctx;
            ctx.fillStyle = '#fff';
            ctx.strokeStyle = 'hsla(0,0%,20%,1)';
            ctx.textBaseLine = 'middle';
            ctx.font = '25px arial';//左侧标注字大小
    //标题
            ctx.clearRect(0, 0, this.W, this.H);
            if (this.title) {
                ctx.save();
                ctx.textAlign = 'right';
                ctx.font = '50px arial';
                ctx.fillText(this.title, this.W / 1.2, 40);
                ctx.restore();
            }
            ctx.save();
            for (var i = 0; i < this.legend.length; i++) {
                item = this.legend[i];
                // 画标签
                ctx.textAlign = 'left';
                ctx.fillStyle = item.color;
                ctx.strokeStyle = item.color;
                roundRect(ctx, item.x, item.y, item.w, item.h, item.r);
                ctx.globalAlpha = item.hide ? 0.3 : 1;
                ctx.fill();
                ctx.fillText(item.name, item.x + item.w + 5, item.y + item.h - 5);
            }
            ctx.restore();
        }
    }
    
    var container=document.querySelectorAll('#container>div')
        var event_li=document.querySelectorAll('#tab>li')
        var currentindex=0
        for(var i=0;i<event_li.length;i++){
            event_li[i].num=i
            event_li[i].onclick=function(){
                container[currentindex].style.display='none'
                var index_other=this.num
                container[index_other].style.display='block'
                currentindex=index_other
            }}
    
            var canvas1=document.getElementById('canvas1');
    var canvas2=document.getElementById('canvas2');
    var canvas3=document.getElementById('canvas3');
    
    var pie=new Pie(canvas1);
    pie.init({
        W:540,
        H:460,
        title:'2022年12月1日全国各省办公政策情况',
        toolTip:'访问来源',
        data:[
            {value:0, name:'不采取措施'},
            {value:9, name:'建议关闭/居家办公'},
            {value:7, name:'要求全部关闭'},
            {value:15, name:'要求部分关闭'}
        ]
    });
    
    var pie=new Pie(canvas2);
    pie.init({
        W:540,
        H:460,
        title:'2022年12月1日全国各省学校政策情况',
        toolTip:'访问来源',
        data:[
            {value:0, name:'不采取措施'},
            {value:11, name:'建议关闭'},
    
            {value:8, name:'要求全部关闭'},
                    {value:12, name:'要求部分关闭'}
        ]
    });
    
    var pie=new Pie(canvas3);
    pie.init({
        W:540,
        H:460,
        title:'2022年12月1日全国各省疫苗接种情况',
        toolTip:'访问来源',
        data:[
            {value:30, name:'比例大于50%'},
            {value:0, name:'比例小于50%'}
        ]
    });})
    </script>
