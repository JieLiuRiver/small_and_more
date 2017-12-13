## 千位分隔符

      #方法1
      String(123456789).replace(/(\d)(?=(\d{3})+$)/g, "$1,");
      #方法2
      (123456789).toLocaleString('en-US');

## css3实现圆环

[点击查看demo](http://www.zhangxinxu.com/study/201711/colorful-circle-by-css-linear-gradient.html)


## canvas实现弹幕
[点击查看demo](http://www.zhangxinxu.com/study/201709/canvas-barrage-static-loop-demo.html)
```

// 弹幕数据
var dataBarrage = [{
	value: '使用的是静态死数据',
	color: 'blue',
	range: [0, 0.5]
}, {
	value: '随机循环播放',
	color: 'blue',
	range: [0, 0.6]
}, {
	value: '可以控制区域和垂直分布范围',
	color: 'blue',
	range: [0, 0.5]
}, {
	value: '字体大小和速度在方法内设置',
	color: 'black',
	range: [0.1, 1]
}, {
	value: '适合用在一些静态页面上',
	color: 'black',
	range: [0.2, 1]
}, {
	value: '基于canvas实现',
	color: 'black',
	range: [0.2, 0.9]
}, {
	value: '因此IE9+浏览器才支持',
	color: 'black',
	range: [0.2, 1]
}, {
	value: '可以设置边框颜色',
	color: 'black',
	range: [0.2, 1]
}, {
	value: '文字颜色默认都是白色',
	color: 'black',
	range: [0.2, 0.9]
}, {
	value: '若文字颜色不想白色',
	color: 'black',
	range: [0.2, 1]
}, {
	value: '需要自己调整下JS',
	color: 'black',
	range: [0.6, 0.7]
}, {
	value: '如果需要的是真实和视频交互的弹幕',
	color: 'black',
	range: [0.2, 1]
}, {
	value: '可以回到原文',
	color: 'black',
	range: [0, 0.9]
}, {
	value: '查看另外一个demo',
	color: 'black',
	range: [0.7, 1]
}, {
	value: '下面就是占位弹幕了',
	color: 'black',
	range: [0.7, 0.95]
}, {
	value: '前方高能预警！！！',
	color: 'orange',
	range: [0.5, 0.8]
}, {
	value: '前方高能预警！！！',
	color: 'orange',
	range: [0.5, 0.9]
}, {
	value: '前方高能预警！！！',
	color: 'orange',
	range: [0, 1]
}, {
	value: '前方高能预警！！！',
	color: 'orange',
	range: [0, 1]
}];

// 弹幕方法
var canvasBarrage = function (canvas, data) {	
	if (!canvas || !data || !data.length) {
		return;
	}
	if (typeof canvas == 'string') {
		canvas = document.querySelector(canvas);
		canvasBarrage(canvas, data);
		return;
	}
	var context = canvas.getContext('2d');
	canvas.width = canvas.clientWidth;
	canvas.height = canvas.clientHeight;

	// 存储实例
	var store = {};

	// 字号大小
	var fontSize = 28;

	// 实例方法
	var Barrage = function (obj, index) {
		// 随机x坐标也就是横坐标，对于y纵坐标，以及变化量moveX
		this.x = (1 + index * 0.1 / Math.random()) * canvas.width;
		this.y = obj.range[0] * canvas.height + (obj.range[1] - obj.range[0]) * canvas.height * Math.random() + 36;
		if (this.y < fontSize) {
			this.y = fontSize;
		} else if (this.y > canvas.height - fontSize) {
			this.y = canvas.height - fontSize;
		}
		this.moveX = 1 + Math.random() * 3;

		this.opacity = 0.8 + 0.2 * Math.random();
		this.params = obj;
		
		this.draw = function () {
			var params = this.params;
			// 根据此时x位置绘制文本
			context.strokeStyle = params.color;
			context.font = 'bold ' + fontSize + 'px "microsoft yahei", sans-serif';
			context.fillStyle = 'rgba(255,255,255,'+ this.opacity +')';
			context.fillText(params.value, this.x, this.y);
			context.strokeText(params.value, this.x, this.y);
		};
	};

	data.forEach(function (obj, index) {
		store[index] = new Barrage(obj, index);
	});

	// 绘制弹幕文本
	var draw = function () {
		for (var index in store) {
			var barrage = store[index];
			// 位置变化
			barrage.x -= barrage.moveX;
			if (barrage.x < -1 * canvas.width * 1.5) {
				// 移动到画布外部时候从左侧开始继续位移
				barrage.x = (1 + index * 0.1 / Math.random()) * canvas.width;
				barrage.y = (barrage.params.range[0] + (barrage.params.range[1] - barrage.params.range[0]) * Math.random()) * canvas.height;
				if (barrage.y < fontSize) {
					barrage.y = fontSize;
				} else if (barrage.y > canvas.height - fontSize) {
					barrage.y = canvas.height - fontSize;
				}
				barrage.moveX = 1 + Math.random() * 3;
			}
			// 根据新位置绘制圆圈圈
			store[index].draw();
		}
	};

	// 画布渲染
	var render = function () {
		// 清除画布
		context.clearRect(0, 0, canvas.width, canvas.height);
		
		// 绘制画布上所有的圆圈圈
		draw();

		// 继续渲染
		requestAnimationFrame(render);
	};

	render();
};

canvasBarrage('#canvasBarrage', dataBarrage);
```

[配合视频完整demo](http://www.zhangxinxu.com/study/201709/canvas-barrage-video-demo.html)


## 百分比padding与固定比例图片自适应布局
[点击查看demo](http://www.zhangxinxu.com/study/201708/percent-padding-auto-layout.html)



## 使用canvas在前端压缩图片实例页面
[查看demo](http://www.zhangxinxu.com/study/201707/js-compress-image-before-upload.html)
```
HTML代码：
<input id="file" type="file">
JS代码：
var eleFile = document.querySelector('#file');

// 压缩图片需要的一些元素和对象
var reader = new FileReader(), img = new Image();

// 选择的文件对象
var file = null;

// 缩放图片需要的canvas
var canvas = document.createElement('canvas');
var context = canvas.getContext('2d');

// base64地址图片加载完毕后
img.onload = function () {
    // 图片原始尺寸
    var originWidth = this.width;
    var originHeight = this.height;
    // 最大尺寸限制
    var maxWidth = 400, maxHeight = 400;
    // 目标尺寸
    var targetWidth = originWidth, targetHeight = originHeight;
    // 图片尺寸超过400x400的限制
    if (originWidth > maxWidth || originHeight > maxHeight) {
        if (originWidth / originHeight > maxWidth / maxHeight) {
            // 更宽，按照宽度限定尺寸
            targetWidth = maxWidth;
            targetHeight = Math.round(maxWidth * (originHeight / originWidth));
        } else {
            targetHeight = maxHeight;
            targetWidth = Math.round(maxHeight * (originWidth / originHeight));
        }
    }
        
    // canvas对图片进行缩放
    canvas.width = targetWidth;
    canvas.height = targetHeight;
    // 清除画布
    context.clearRect(0, 0, targetWidth, targetHeight);
    // 图片压缩
    context.drawImage(img, 0, 0, targetWidth, targetHeight);
    // canvas转为blob并上传
    canvas.toBlob(function (blob) {
        // 图片ajax上传
        var xhr = new XMLHttpRequest();
        // 文件上传成功
        xhr.onreadystatechange = function() {
            if (xhr.status == 200) {
                // xhr.responseText就是返回的数据
            }
        };
        // 开始上传
        xhr.open("POST", 'upload.php', true);
        xhr.send(blob);    
    }, file.type || 'image/png');
};

// 文件base64化，以便获知图片原始尺寸
reader.onload = function(e) {
    img.src = e.target.result;
};
eleFile.addEventListener('change', function (event) {
    file = event.target.files[0];
    // 选择的文件是图片
    if (file.type.indexOf("image") == 0) {
        reader.readAsDataURL(file);    
    }
});
```
[FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsDataURL)

## 使用JS把文本内容作为html文件下载

[查看demo](http://www.zhangxinxu.com/study/201707/js-text-download-as-html-file.html)
```
var eleTextarea = document.querySelector('textarea');
var eleButton = document.querySelector('input[type="button"]');

// 下载文件方法
var funDownload = function (content, filename) {
    var eleLink = document.createElement('a');
    eleLink.download = filename;
    eleLink.style.display = 'none';
    // 字符内容转变成blob地址
    var blob = new Blob([content]);
    eleLink.href = URL.createObjectURL(blob);
    // 触发点击
    document.body.appendChild(eleLink);
    eleLink.click();
    // 然后移除
    document.body.removeChild(eleLink);
};

if ('download' in document.createElement('a')) {
    // 作为test.html文件下载
    eleButton.addEventListener('click', function () {
        funDownload(eleTextarea.value, 'test.html');    
    });
} else {
    eleButton.onclick = function () {
        alert('浏览器不支持');    
    };
}
```
[关于Blog](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

## JS canvas水印图片合成
[查看demo](http://www.zhangxinxu.com/study/201705/js-canvas-image-watermark-synthesis.html)
```
CSS代码：
.clip {
    position: absolute;
    clip: rect(0 0 0 0);
}
HTML代码：
<input type="file" id="uploadFile" class="clip" accept="image/*">
<label class="ui-button ui-button-primary" for="uploadFile">选择图片</label>
<img id="imgCover" src="./watermark.png" class="clip">
<p id="imgUploadX"></p>
JS代码：
var eleUploadFile = document.getElementById('uploadFile');
var eleImgCover = document.getElementById('imgCover');
var eleImgUploadX = document.getElementById('imgUploadX');

if (history.pushState) {
    eleUploadFile.addEventListener('change', function (event) {
        var reader = new FileReader();
        var file = event.target.files[0] || event.dataTransfer.files[0];
        reader.onload = function(e) {
          var base64 = e.target.result;
          if (base64.length > 1024 * 50) {
              alert('图片尺寸请小于50K');
              return;
          } else {
              // 使用canvas合成图片，并base64化
              imgTogether(base64, function (url) {
                  // 尺寸
                  var size = 180 / (window.devicePixelRatio || 1);
                  // 预览
                  eleImgUploadX.innerHTML = '<img src="'+ url +'" width="'+ size +'" height="'+ size +'">';
              });
          }
        };
        reader.readAsDataURL(file);
    });
    
    // canvas图片合成
    var imgTogether = function (url, callback) {
        var canvas = document.createElement('canvas');
        var size = 180;
        canvas.width = size;
        canvas.height = size;

        var context = canvas.getContext('2d');

        // 这是上传图像
        var imgUpload = new Image();
        imgUpload.onload = function () {
            // 绘制
            context.drawImage(imgUpload, 0, 0, size, size, 0,0, size, size);
            // 再次绘制
            context.drawImage(eleImgCover, 0, 0, size, size, 0,0, size, size);
            // 回调
            callback(canvas.toDataURL('image/png'));
        };
        imgUpload.src = url;
    };
} else if (eleImgUploadX) {
    eleImgUploadX.className = 'remind';
    eleImgUploadX.innerHTML = '本演示IE10+下才有效果';
}
```

## 移动端事件
```

var Tween = {
	linear: function (t, b, c, d){
		return c*t/d + b;
	},
	easeIn: function(t, b, c, d){
		return c*(t/=d)*t + b;
	},
	easeOut: function(t, b, c, d){
		return -c *(t/=d)*(t-2) + b;
	},
	easeBoth: function(t, b, c, d){
		if ((t/=d/2) < 1) {
			return c/2*t*t + b;
		}
		return -c/2 * ((--t)*(t-2) - 1) + b;
	},
	easeInStrong: function(t, b, c, d){
		return c*(t/=d)*t*t*t + b;
	},
	easeOutStrong: function(t, b, c, d){
		return -c * ((t=t/d-1)*t*t*t - 1) + b;
	},
	easeBothStrong: function(t, b, c, d){
		if ((t/=d/2) < 1) {
			return c/2*t*t*t*t + b;
		}
		return -c/2 * ((t-=2)*t*t*t - 2) + b;
	},
	elasticIn: function(t, b, c, d, a, p){
		if (t === 0) { 
			return b; 
		}
		if ( (t /= d) == 1 ) {
			return b+c; 
		}
		if (!p) {
			p=d*0.3; 
		}
		if (!a || a < Math.abs(c)) {
			a = c; 
			var s = p/4;
		} else {
			var s = p/(2*Math.PI) * Math.asin (c/a);
		}
		return -(a*Math.pow(2,10*(t-=1)) * Math.sin( (t*d-s)*(2*Math.PI)/p )) + b;
	},
	elasticOut: function(t, b, c, d, a, p){
		if (t === 0) {
			return b;
		}
		if ( (t /= d) == 1 ) {
			return b+c;
		}
		if (!p) {
			p=d*0.3;
		}
		if (!a || a < Math.abs(c)) {
			a = c;
			var s = p / 4;
		} else {
			var s = p/(2*Math.PI) * Math.asin (c/a);
		}
		return a*Math.pow(2,-10*t) * Math.sin( (t*d-s)*(2*Math.PI)/p ) + c + b;
	},    
	elasticBoth: function(t, b, c, d, a, p){
		if (t === 0) {
			return b;
		}
		if ( (t /= d/2) == 2 ) {
			return b+c;
		}
		if (!p) {
			p = d*(0.3*1.5);
		}
		if ( !a || a < Math.abs(c) ) {
			a = c; 
			var s = p/4;
		}
		else {
			var s = p/(2*Math.PI) * Math.asin (c/a);
		}
		if (t < 1) {
			return - 0.5*(a*Math.pow(2,10*(t-=1)) * 
					Math.sin( (t*d-s)*(2*Math.PI)/p )) + b;
		}
		return a*Math.pow(2,-10*(t-=1)) * 
				Math.sin( (t*d-s)*(2*Math.PI)/p )*0.5 + c + b;
	},
	backIn: function(t, b, c, d, s){
		if (typeof s == 'undefined') {
		   s = 1.70158;
		}
		return c*(t/=d)*t*((s+1)*t - s) + b;
	},
	backOut: function(t, b, c, d, s){
		if (typeof s == 'undefined') {
			s = 2.70158;  //回缩的距离
		}
		return c*((t=t/d-1)*t*((s+1)*t + s) + 1) + b;
	}, 
	backBoth: function(t, b, c, d, s){
		if (typeof s == 'undefined') {
			s = 1.70158; 
		}
		if ((t /= d/2 ) < 1) {
			return c/2*(t*t*(((s*=(1.525))+1)*t - s)) + b;
		}
		return c/2*((t-=2)*t*(((s*=(1.525))+1)*t + s) + 2) + b;
	},
	bounceIn: function(t, b, c, d){
		return c - Tween['bounceOut'](d-t, 0, c, d) + b;
	},       
	bounceOut: function(t, b, c, d){
		if ((t/=d) < (1/2.75)) {
			return c*(7.5625*t*t) + b;
		} else if (t < (2/2.75)) {
			return c*(7.5625*(t-=(1.5/2.75))*t + 0.75) + b;
		} else if (t < (2.5/2.75)) {
			return c*(7.5625*(t-=(2.25/2.75))*t + 0.9375) + b;
		}
		return c*(7.5625*(t-=(2.625/2.75))*t + 0.984375) + b;
	},      
	bounceBoth: function(t, b, c, d){
		if (t < d/2) {
			return Tween['bounceIn'](t*2, 0, c, d) * 0.5 + b;
		}
		return Tween['bounceOut'](t*2-d, 0, c, d) * 0.5 + c*0.5 + b;
	}
};
function css(element, attr , val){
	if(attr=='scale'|| attr=='rotate'|| attr=='rotateX'|| attr=='rotateY'|| attr=='rotateZ'|| attr=='scaleX'|| attr=='scaleY'|| attr=='translateY'|| attr=='translateX'|| attr=='translateZ' || attr=='skewX' || attr=='skewY'||attr=='skewZ'){
		return setTransform(element, attr , val);
	}
	if(arguments.length == 2){
		var val = element.currentStyle?element.currentStyle[attr]:getComputedStyle(element)[attr];
		if(attr=='opacity'){
			val = Math.round(val*100);
		}
		return parseFloat(val);
	} else {
		switch(attr){
			case 'width':
			case 'height':
			case 'paddingLeft':
			case 'paddingTop':
			case 'paddingRight':
			case 'paddingBottom':
			case 'borderWidth':
			case 'borderLeftWidth':
			case 'borderRightWidth':
			case 'borderTopWidth':
			case 'borderBottomWidth':
				val = val < 0 ? 0 : val;
			case 'left':
			case 'top':
			case 'marginLeft':
			case 'marginTop':
			case 'marginRight':
			case 'marginBottom':
				element.style[attr] = val +"px";
				break;
			case 'opacity':
				element.style.filter= "alpha(opacity:"+val+")";
				element.style.opacity= val/100;
				break;	
			default:
				element.style[attr]=val;	
		}
	}
}
function setTransform(element,attr,val){
	if(!element["transform"]){
		element["transform"] = {};
	}
	if(typeof val == "undefined"){
		val = element["transform"][attr];
		if(typeof val == "undefined"){
			val = 0;
			element["transform"][attr] = 0;
		}
		return parseFloat(val);
	} else {
		var str = "";
		element["transform"][attr] = val;
		for(var s in element["transform"])	 {
			switch(s){
				case 'skewX':
				case 'skewY':
				case 'skewZ':
				case 'rotate':
				case 'rotateX':
				case 'rotateY':
				case 'rotateZ':
					str += s+"("+element["transform"][s]+"deg) ";
					break;
				case 'translateX':
				case 'translateY':
				case 'translateZ':	
					str += s+"("+element["transform"][s]+"px) ";
					break;
				default:
					str += s+"("+element["transform"][s]/100+") ";
			}
		}
		element.style.MozTransform = element.style.msTransform  = element.style.WebkitTransform = element.style.transform = str;
	}
}
function MTween(el, target, time, type, callBack){
	var t = 0;
	var b = {};
	var c = {};
	var d = time / 20;
	for(var s in target){ 
		b[s] = css(el, s); 
		c[s] = target[s] - b[s];
	}
	clearInterval(el.timer); 
	el.timer = setInterval(
		function(){
			t++;
			if(t>=d){
				clearInterval(el.timer);
				callBack&&callBack.call(el);
			}
			for(var s in b){
				var val = (Tween[type](t,b[s],c[s],d)).toFixed(2);
				css(el, s, val);
			}
		},
		20
	);
}
function MScroll(init){
	var _this = this;
	this.showBar = false;
	this.dir = "y";
	this.isOver = true;
	this.offMove = false;
	this.offScroll = false;
	for(var s in init){
		this[s] = init[s];
	}
	var _this = this;
	this.Scroll =  this.element.children[0]; 
	this.startPage = {x:0,y:0}; 
	this.startTranslate = {x:0,y:0};
	this.iScroll = {x:0,y:0}; 
	this.lastTime = 0; 
	this.lastTranslate = {x:0,y:0}; 
	this.timeDis = 0;  
	this.translateDis = {x:0,y:0};
	this.backout = 100; 
	this.timer = 0; 
	var isMove = false; 
	this.isScroll = {x:false,y:false};
	css(this.Scroll,"translateZ",0);
	if(this.showBar){ 
		if(this.dir == "y"){
			ceateY();
		} else if(this.dir == "x") {
			ceateX();
		} else {
			ceateX()
			ceateY();
		}	
		function ceateX(){
			_this.scrollXBar = document.createElement("div");
		 	_this.scrollXBar.style.cssText="height:4px;position:absolute;background:rgba(0,0,0,.5);left:0;bottom:0;border-radius:2px;min-width:4px;opacity:0; transition:.2s opacity;";
			_this.element.appendChild(_this.scrollXBar);
		}
		function ceateY(){
			_this.scrollYBar = document.createElement("div");
		 	_this.scrollYBar.style.cssText="width:4px;position:absolute;background:rgba(0,0,0,.5);right:0;top:0;border-radius:2px;min-height:4px; opacity:0; transition:.2s opacity;";
			_this.scrollYBar.className = "scrollBar";
			_this.element.appendChild(_this.scrollYBar);
		}
	}
	this.reSize();
	this.element.addEventListener("touchstart",
		function(e){
			isMove = false;
			_this.toStart(e);			
		},
		false
	);
	this.element.addEventListener("touchmove",
		function(e){
			isMove = true;
			_this.toMove(e);
			e.preventDefault();
		},
		false
	);
	this.element.addEventListener("touchend",
		function(e){
			if(!isMove){
				return;
			}
			_this.toEnd(e);
		},
		false
	);
}
MScroll.prototype = {
	toStart: function(e){ 
		clearTimeout(this.timer);
		this.isOverMove = false;
		this.overMove();
		this.onscrollstart &&  this.onscrollstart();
		this.startPage = {x:e.changedTouches[0].pageX,y:e.changedTouches[0].pageY};
		this.startTranslate.x = this.iScroll.x;
		this.startTranslate.y = this.iScroll.y;
		this.lastTime = new Date().getTime(); 
		this.lastTranslate = {x:this.iScroll.x,y:this.iScroll.y};
		this.timeDis = 0;
		this.translateDis = {x:0,y:0};
		if(this.dir == "y"){
			this.isScroll.y = true;
		} else if(this.dir == "x") {
			this.isScroll.x = true;
		} else {
			this.isScroll = {x:true,y:true};
		} 
		if(this.showBar){
			this.scrollXBar&&(this.scrollXBar.style.opacity = 1);
			this.scrollYBar&&(this.scrollYBar.style.opacity = 1);
		}
	},
	toMove: function(e){
		if(this.offScroll){
			return;
		}
		var _this = this;
		var nowPage = {x:e.changedTouches[0].pageX,y:e.changedTouches[0].pageY};
		var nowTime = new Date().getTime();
		var disX =  nowPage.x - this.startPage.x;
		var disY = nowPage.y - this.startPage.y;
		if(Math.abs(disX) - Math.abs(disY) > 5){
			this.isScroll.y = false;
		} else if(Math.abs(disY) - Math.abs(disX) > 5){
			this.isScroll.x = false;
		}
		this.isScroll.y&&setScroll("y");
		this.isScroll.x&&setScroll("x");
		function setScroll(dir){
			_this.iScroll[dir] = _this.startTranslate[dir] + (nowPage[dir] - _this.startPage[dir]);
			if(_this.minTranslate[dir] -  _this.backout > _this.iScroll[dir] && _this.isOver){
				_this.iScroll[dir] = _this.minTranslate[dir] - _this.backout;
			}
			if(_this.iScroll[dir] > _this.backout  && _this.isOver){
				_this.iScroll[dir] = _this.backout;
			}
			_this.timeDis = nowTime - _this.lastTime; 
			_this.lastTime = nowTime;
			_this.translateDis[dir] = _this.iScroll[dir] - _this.lastTranslate[dir]; 
			_this.lastTranslate[dir] =  _this.iScroll[dir];
		}
		this.setTranslate();
	},
	toEnd: function(e){ 
		var _this = this;
		var type = "cubic-bezier(.1,.69,.1,1)";
		var target = {x:getTarget("x"),y:getTarget("y")};
		function getTarget(dir)
		{	
			var speed = _this.translateDis[dir] / _this.timeDis*10;
			speed = _this.timeDis == 0 ? 0 : speed; 
			speed = Math.abs(speed) > 5? speed*20 : speed*5;
			time = parseInt(Math.abs(speed)*.7);
			var target = speed + _this.iScroll[dir];
			if( _this.minTranslate[dir] > target  && _this.isOver ){
				target = _this.minTranslate[dir];
				type = "cubic-bezier(.31,1.23,.59,1.13)";
			}
			if(target > 0  && _this.isOver){
				target = 0;
				type = "cubic-bezier(.31,1.23,.59,1.13)";
			}
			return target;
		}
		this.timer = setTimeout(
			function(){
				_this.iScroll.x = target.x;
				_this.iScroll.y = target.y; 
				_this.move(type,time);
			},30
		);
		this.onscrollend && this.onscrollend();
	},
	setTranslate: function(){
		this.onscroll&&this.onscroll();
		if(this.offMove){
			return;
		}
		this.scrollXBar&&css(this.scrollXBar,"translateX", -this.iScroll.x * this.scale.x)
		css(this.Scroll,"translateX",this.iScroll.x);
		this.scrollYBar&&css(this.scrollYBar,"translateY", -this.iScroll.y * this.scale.y)
		css(this.Scroll,"translateY",this.iScroll.y);
	},
	move: function(type,time){
		var _this = this;
		if(typeof time == "undefined"){
			time = 300;
		}
		time = time<300?300:time;
		this.Scroll.style.WebkitTransitionDuration = this.Scroll.style.transitionDuration =  time+"ms";
		this.Scroll.style.WebkitTransitionTimingFunction =  this.Scroll.style.transitionTimingFunction = type;
		this.isOverMove = true;
		if(this.scrollXBar)
		{
			this.scrollXBar.style.WebkitTransition = this.scrollXBar.style.transition = time+"ms "+type;
		}
		if(this.scrollYBar)
		{
			this.scrollYBar.style.WebkitTransition = this.scrollYBar.style.transition = time+"ms "+type;
		}
		this.timer = setTimeout(
			function (){
				_this.overMove();
			},
			time
		);
		this.setTranslate();
	},
	reSize: function(){
		this.minTranslate = {
			x:this.element.clientWidth - this.Scroll.offsetWidth,
			y:this.element.clientHeight - this.Scroll.offsetHeight
		};
		if(this.showBar){
			this.scale = {
				x:this.element.clientWidth  / this.Scroll.offsetWidth,
				y:this.element.clientHeight  / this.Scroll.offsetHeight
			}
			if(this.dir == "y"){
				this.scrollYBar.style.height = this.element.clientHeight * this.scale.y +"px";
			} else if(this.dir == "x") {
				this.scrollXBar.style.width = this.element.clientWidth * this.scale.x +"px";
			} else {
				this.scrollXBar.style.width = this.element.clientWidth * this.scale.x +"px";
				this.scrollYBar.style.height = this.element.clientHeight * this.scale.y +"px";
			}
		}
		this.setTranslate();
	},
	overMove:function (){
		this.scrollOver &&this.isOverMove && this.scrollOver();
		this.isOverMove = false;
		this.Scroll.style.WebkitTransitionDuration = this.Scroll.style.transitionDuration =  			0+"ms";
		if(this.scrollXBar)
		{
			this.scrollXBar.style.WebkitTransition = this.scrollXBar.style.transition = "200ms opacity";
			this.scrollXBar.style.opacity = 0;
		}
		if(this.scrollYBar)
		{
			this.scrollYBar.style.WebkitTransition = this.scrollYBar.style.transition = "200ms opacity";
			this.scrollYBar.style.opacity = 0;
		}	
	} 
};
function TouchEevent(obj){
	var _this = this;
	this.obj = obj;
	for(var i=0; i<this.obj.length; i++){
		this.obj[i].touches={x:0,y:0};
		this.obj[i].isMove = false;
		this.obj[i].index = i;
		this.obj[i].addEventListener("touchstart",
			function(e){
				_this.fnStart(e,this.index);
			},
			false
		);
		this.obj[i].addEventListener("touchmove",
			function(e){
				this.isMove = true;
			},
			false
		);
		this.obj[i].addEventListener("touchend",
			function(e){
				_this.fnEnd(e,this.index);
			},
			false
		);
	}
}
TouchEevent.prototype = {
	fnStart: function(e,index){
			this.obj[index].touches={x:e.changedTouches[0].pageX,y:e.changedTouches[0].pageY};
			this.obj[index].isMove = false;
	},
	fnEnd: function(e,index){
			var nowTouches = {x:e.changedTouches[0].pageX,y:e.changedTouches[0].pageY};
			var disX = nowTouches.x -this.obj[index].touches.x;
			var disY = nowTouches.y - this.obj[index].touches.y;
			if(disX != 0 || disY != 0){
				if(this.swipe){
					this.swipe.call(this.obj[index]);
				}
				if(disX > 10 && this.swipeRight){
						this.swipeRight.call(this.obj[index]);
				}
				if(disX < -10 && this.swipeLeft){
					this.swipeLeft.call(this.obj[index]);
				}
				if(disY < -10 && this.swipeUp){
						this.swipeUp.call(this.obj[index]);
				}
				if(disY > 10 && this.swipeDown){
					this.swipeDown.call(this.obj[index]);
				}
			}
			if(!this.obj[index].isMove && this.tap){
				this.tap.call(this.obj[index],e);
			}
	},
	tap: function(fn){
		this.tap = fn;
	},
	swipe: function(fn){
		this.swipe = fn;
	},
	swipeLeft: function(fn){
		this.swipeLeft = fn;
	},
	swipeRight: function(fn){
		this.swipeRight = fn;
	},
	swipeUp: function(fn){
		this.swipeUp = fn;
	},
	swipeDown: function(fn){
		this.swipeDown = fn;
	}
};
function MTouch(obj){
	if(typeof obj == "string"){
		obj = document.querySelectorAll(obj);
	}
	if(typeof obj.length == "number")
	{
		return new TouchEevent(obj);
	}
	return new TouchEevent([obj]);
}
function getIos()
{
      var u = navigator.userAgent;
      return !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
}
function MSetGravity(fn3){
	var mEvent={
		x:0,
		y:0,
		z:0,
		gamma:0,
		alpha:0,
		beta:0
	};
	window.addEventListener("devicemotion",fn,false);
	window.addEventListener("deviceorientation",fn2,false);
	function fn(e){
		var oMotion=e.accelerationIncludingGravity;
		if(getIos()){
			mEvent.x = oMotion.x;
			mEvent.y = oMotion.y;
			mEvent.z = oMotion.z;
		} else {
			mEvent.x = -oMotion.x;
			mEvent.y = -oMotion.y;
			mEvent.z = -oMotion.z;
		}
		fn3&&fn3(mEvent);
	}
	function fn2(e){
		mEvent.gamma = e.gamma;
		mEvent.beta = e.beta;
		mEvent.alpha = e.alpha;
	}
}
function MGravityShake(callBack){
	var SHAKERANGE = 1500;
	var lastX = 0;
	var lastY = 0;
	var lastZ = 0;
	var lastTime = 0;
	var isShanke = false;
	window.addEventListener("devicemotion",fnShake,false);
	function fnShake(e){
		var acceleratio = e.accelerationIncludingGravity;
		var nowTime = new Date().getTime();
		var disTime = nowTime - lastTime; 
		if(disTime > 100){
			var x = acceleratio.x;
			var y = acceleratio.y;
			var z = acceleratio.z;
			var speed = (x+y+z - lastX - lastY - lastZ)/disTime * 5000;
			if(speed > SHAKERANGE){
				isShanke = true;
			}
			if(isShanke&&speed<200){
				isShanke = false;
				callBack&&callBack();
			}
			lastX = x;
			lastY = y;
			lastZ = z;
			lastTime = nowTime;
		}
	}
}

function getDistance(p1, p2) {
    var x = p2.pageX - p1.pageX,
        y = p2.pageY - p1.pageY;
    return Math.sqrt((x * x) + (y * y));
}

function getAngle(p1, p2) {
    var x = p1.pageX - p2.pageX,
        y = p1.pageY- p2.pageY;
    return Math.atan2(y, x) * 180 / Math.PI;
}

function MSetGesture(el){
	var obj = {};
	var isGesTure = false;
	var startPinter = [];
	el.addEventListener("touchstart",
		function(e){
			if(e.touches.length >= 2){
				isGesTure = true;
				startPinter = e.touches;
				obj.gesturestart&&obj.gesturestart.call(el);
			}
		},
		false
	);
	document.addEventListener("touchmove",
		function(e){
			if(e.touches.length >= 2&&isGesTure){
				var nowPinter = e.touches;
				var scale = getDistance(nowPinter[0], nowPinter[1]) / getDistance(startPinter[0], startPinter[1]);
				var rotate =  getAngle(nowPinter[0], nowPinter[1]) -  getAngle(startPinter[0], startPinter[1]) 
				e.scale = scale; 
				e.rotation = rotate;
				obj.gesturemove&&obj.gesturemove.call(el,e);
			}
		},
		false
	)
	document.addEventListener("touchend",
		function(){
			if(isGesTure){
				isGesTure = false
				obj.gestureend&&obj.gestureend.call(el);
			}
		},
		false
	);
	return obj;
}
```
