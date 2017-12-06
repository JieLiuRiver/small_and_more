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
