# lunbotu
原生JS实现轮播图
Html布局
  父容器container存放所有内容，子容器list存在图片。子容器buttons存放按钮圆点。两个a标签存放左右箭头

```html
 <div id="container">
        <!-- 放图片 -->
        <div id="list" style="left:0px;">
            <img src="./images/1.jpg" alt="1">
            <img src="./images/2.jpg" alt="2">
            <img src="./images/3.jpg" alt="3">
            <img src="./images/4.jpg" alt="4">
        </div>
        <!-- 放圆点 -->
        <div id="buttons">
            <span class="on"></span>
            <span></span>
            <span></span>
            <span></span>
        </div>
        <!-- 左右箭头 -->
        <a href="javascript:;" id="prev" class="arrow">&lt;</a>
        <a href="javascript:;" id="next" class="arrow">&gt;</a>
    </div>
```

CSS布局

```css
	    * {
            margin: 0;
            padding: 0;
            text-decoration: none;
        }
        body {
            padding: 20px;
        }
        #container {
            position: relative;
            width: 560px;
            height: 300px;
            border: 1px solid rebeccapurple;
            /* 盒子大小为一张图片的大小，超出部分隐藏 */
            overflow: hidden;
            margin: 0 auto;
            /* 水平居中*/
        }
        /* 图片盒子要足够宽，放得下所有图片 */
        #list {
            position: absolute;
            z-index: 1;
            width: 3360px;
            /* 每张图片宽560px,高300px;六张图片共3000px */
            height: 300px;
        }
        #list img {
            /* 所有图片左浮动 */
            float: left;
            width: 560px;
            height: 300px;
        }
        #buttons {
            position: absolute;
            /* 水平居中*/
            left: 50%;
            transform: translateX(-50%);
            bottom: 20px;
            z-index: 2;
            height: 10px;
            width: 100px;
        }
        #buttons span {
            float: left;
            margin-right: 5px;
            width: 10px;
            height: 10px;
            border: 1px solid #fff;
            border-radius: 50%;
            background: #333;
            cursor: pointer;
        }
        /* 圆点选中的样式 */
        #buttons .on {
            background: orangered;
        }
        /* 箭头样式 */
        .arrow {
            position: absolute;
            z-index: 2;
            /* 一开始箭头默认不显示 */
            display: none;
            width: 40px;
            height: 40px;
            font-size: 36px;
            font-weight: bold;
            line-height: 39px;
            color: #fff;
            background-color: RGBA(0, 0, 0, .3);
            cursor: pointer;
            /* 垂直居中*/
            top: 50%;
            transform: translateY(-50%);
        }
        .arrow:hover {
            background-color: RGBA(0, 0, 0, .7);
        }
        /* 鼠标移动上去显示 */
        #container:hover .arrow {
            display: block;
        }
        #prev {
            left: 15px;
        }
        #next {
            right: 15px;
        }
```

JS实现：

1.实现左右箭头切换图片

```javascript
		var list = document.getElementById('list');
        var prev = document.getElementById('prev');
        var next = document.getElementById('next');

        // 第一张图片 left:0px;
        // 右移:-560px;-1120px;-1680px;0px;
        // 左移:-1680px;-1120px;-560px;0px;

        function move(offset) {
            //获取的是style.left，是相对左边获取距离，所以第一张图后style.left都为负值，
            //且style.left获取的是字符串，需要用parseInt()取整转化为数字。
            var newLeft = parseInt(list.style.left) + offset;
            list.style.left = newLeft + 'px';
            //和第一张图片位置比较,也就是到了最后一张图片
            if (newLeft < -1680) {
                // 归位到第一张图片
                list.style.left = 0 + 'px';
            }
            //和最后一张图片位置比较，也就是当前为第一张图片（left:0px;）左移后到最后一张图
            if (newLeft > 0) {
                list.style.left = -1680 + 'px';
            }
        }
        prev.onclick = function () {
            move(560);  //560是一张图片的宽度
        }
        next.onclick = function () {
            move(-560);
        }

```

2.实现自动循环播放，利用setInterval

```javascript
 		// 循环滚动
        var timer;
        function play() {
            timer = setInterval(function () {
                next.onclick();
            }, 2000)
        }
        play();

```

3.实现鼠标移入，图片停住

```javascript
		// 鼠标移入，图片停住
        var container = document.getElementById('container');
        function stop() {
            clearInterval(timer);
        }
        container.onmouseover = stop;
        container.onmouseout = play;
```

4.实现圆点自动选择当前图片

```javascript
		// 圆点的移动
        var buttons = document.getElementsByTagName('span');
		var index = 1;//下标
        function buttonShow() {
            for (var i = 0; i < buttons.length; i++) {
                // 先清除之前的样式
                if (buttons[i].className == 'on') {
                    buttons[i].className = '';
                }
            }
            // 再给当前圆点添加样式
            buttons[index-1].className = 'on';
        }

		//点击上或者下一张图片改为
		prev.onclick = function () {
            index = index - 1;
            if (index < 1) {
                index = 4;
            }
            buttonShow();
            move(560);
        }
        next.onclick = function () {
            index = index + 1;
            if (index > 4) {
                index = 1;
            }
            buttonShow();
            move(-560);
        }
```

5.实现点击圆点切换到相应图片，通过偏移量找到对应图片

```javascript
		// 通过鼠标任意点击其中一个小圆点，切换到相应的图片
		//通过块级作用域获取到正确的下标，如果用var则每次都打印i为4
        for (let i = 0; i < buttons.length; i++) {
            buttons[i].onclick = function () {
                //console.log(i);
                var clickIndex = i + 1;
                var offset = 560 * (index - clickIndex);
                move(offset);
                index = clickIndex;
                buttonShow();
            }
        }
```

```javascript
		for (var i = 0; i < buttons.length; i++) {
            //不使用块级作用域，利用闭包的自执行函数实现
            (function (i) {
                buttons[i].onclick = function () {
                    //console.log(i);
                    var clickIndex = i + 1;
                    var offset = 560 * (index - clickIndex);
                    move(offset);
                    index = clickIndex;
                    buttonShow();
                }
            })(i)
        }
```

