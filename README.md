# lunbotu
原生JS实现轮播图
Html布局
  父容器container存放所有内容，子容器list存在图片。子容器buttons存放按钮圆点。两个a标签存放左右箭头
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
            <span index="1" class="on"></span>
            <span index="2"></span>
            <span index="3"></span>
            <span index="4"></span>
        </div>
        <!-- 左右箭头 -->
        <a href="javascript:;" id="prev" class="arrow">&lt;</a>
        <a href="javascript:;" id="next" class="arrow">&gt;</a>
    </div>
