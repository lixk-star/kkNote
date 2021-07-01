#### 1.盒模型

* 标准盒子模型:box-sizing：border-box：盒子的宽度和高度就是给定的宽度和高度，如果设置了padding和border，将减少内容的宽度和高度。
* box-sizing：content-box：盒子的宽度是**给定的宽度+padding+border**,盒子的高度是**给定的高度 + padding + border**，设置了padding和border也不会减少内容的宽度和高度。

####  2.flex 属性布局

* display:flex; 在父元素设置，子元素受弹性盒影响，flex-direction决定主轴的方向，align-items决定交叉轴的排列方向，justify-content觉得主轴排列方式，默认排成一行，如果超出一行，按比例压缩 flex:1; 子元素设置，设置子元素如何分配父元素的空间，flex:1,子元素宽度占满整个父元素align-items:center 定义子元素在父容器中的对齐方式，center 垂直居中justify-content:center 设置子元素在父元素中居中，前提是子元素没有把父元素占满，让子元素水平居中

#### 3.css中行内元素和行内块元素空白间隙的问题

在html代码中，如果把行内元素或者行内块元素写成下面这样的话，会出现空格的问题：

```
<div class="wrapper">
  <span>我是行内元素</span>
  <span>我是行内元素</span>
  <span>我是行内元素</span>
</div>
.wrapper span {
  /*display: inline-block;*//* 这句代码加不加效果都一致 */
  font-size: 16px;
  background-color: lime;
  color: #fff;
}
```

![image-20210112230530633](E:/gitProject/kkNote/面试题/image/image-20210112230530633.png)

我们代码里面的这几个`span`标签都有换行，这些换行也叫作空文本节点，会被保留为一个空格，所以我们要去掉这个空文本节点带来的问题。

1.给他们的父级元素加上`font-size: 0;`这个属性，就可以解决。原理是：空文本节点也是文本，自然可以被`font-size: 0;`作用到，那么空文本节点自然就没了。ie7级ie版本中不兼容

```
.wrapper {
  font-size:0;letter-spaceing:-4px;/* 去掉空文本节点 */
}
```

> 3，font-size:0，去除换行符间隙，在IE6/7下残留1像素间隙，Chrome浏览器无效，其他浏览器都完美去除；
> 4，letter-spacing负值可以去除所有浏览器的换行符间隙，但是，Opera浏览器下极限是间隙1像素，0像素会反弹，换行符间隙还原。
>
> 推荐解决方法：
>
> ### 父元素中设置 
>
> font-size:0;letter-spaceing:-4px;

2.取消代码换行

这种方法非常直观，但是代码并不美观了。。。而且维护起来也不方便。但是兼容性好。

```html
<div class="wrapper">
  <span>我是行内元素</span><span>我是行内元素</span><span>我是行内元素</span>
</div>
```

3.还有其他的一些方法都类似于第二种方法，就是变相的取消换行（在这里我只说一个吧）：

```html
<div class="wrapper">
  <span>我是行内元素</span
  ><span>我是行内元素</span
  ><span>我是行内元素</span>
</div>
```



#### 4.单行省略 跟 多行省略怎么写？

```css
/*单行省略*/
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap


/*多行省略*/
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;

```



#### 5.web开发中动画效果的实现方法

1. animation+keyframes 、transition  
3. canvas、2D、3D webgl 
4. setInterval()、requestAnimationFrame
6. svg
8. GIF
9. web API animation //  兼容性不好

```css
const element = document.getElementById('some-element-you-want-to-animate');
let start;

function step(timestamp) {
  if (start === undefined)
    start = timestamp;
  const elapsed = timestamp - start;

  //这里使用`Math.min()`确保元素刚好停在200px的位置。
  element.style.transform = 'translateX(' + Math.min(0.1 * elapsed, 200) + 'px)';

  if (elapsed < 2000) { // 在两秒后停止动画
    window.requestAnimationFrame(step);
  }
}

window.requestAnimationFrame(step);
```



#### 6.获取元素的样式信息

获取元素的样式信息，通过elem.style[’属性’] ,只能获取元素内嵌style属性上的生命的css属性，

而不包括来自其他地方声明的样式，如<head>部分的内嵌样式表，或外部样式表；

要获取一个元素的所有css 属性，使用window.getComputedStyle();

```css
let para = document.querySelector('p');
let compStyles = window.getComputedStyle(para);
para.textContent = 'My computed font-size is ' +
    compStyles.getPropertyValue('font-size') +
    ',\nand my computed line-height is ' +
    compStyles.getPropertyValue('line-height') +
    '.';

<style>
  h3::after {
    content: ' rocks!';
  }
</style>

<h3>Generated content</h3>

<script>
  var h3 = document.querySelector('h3');
  var result = getComputedStyle(h3, ':after').content;

  console.log('the generated content is: ', result); // returns ' rocks!'
</script>
```

#### 7.margin-top 百分比是相对于

父元素的width来参考的

#### 8.背景图像百分之百显示

`background-size:100% 100%`

#### 9.css 性能优化

https://www.cnblogs.com/heroljy/p/9412704.html

#### 10.垂直居中

https://juejin.cn/post/6844903839187877895line-height

* 对于绝对定位元素来说
  * 定位参照对象的宽度 = left + right + margin-left + margin-right + 绝对定位元素的实际占用宽度
  * 定位参照对象的高度 = top + bottom + margin-top + margin-bottom + 绝对定位元素的实际占用高度
* 如果希望绝对定位元素的宽高和定位参照对象一样，可以给绝对定位元素设置以下属性
  * left: 0、right: 0、top: 0、bottom: 0、margin:0
* 如果希望绝对定位元素在定位参照对象中居中显示，可以给绝对定位元素设置以下属性
  * left: 0、right: 0、top: 0、bottom: 0、margin: auto
  * 另外，还得设置具体的宽高值（宽高小于定位参照对象的宽高）

1. tranform

   ```css
   .body {
       position: relative;
   }
   .box {
       position: absolute;
       top:50%;
       left:50%;
       tranform:translate(-50%,-50%);   
   }
   //兼容性不好
   ```

   

2. flexbox `align-items或align-content`

   ```
   .use-flexbox{
       display:flex;
       align-items:center;
       justify-content:center;
       width:200px;
       height:150px;
       border:1px solid #000;
   }
   .use-flexbox div{
       width:100px;
       height:50px;
       background:#099;
   }
   
   ```

   

3. 绝对定位: `position:absolute`

   ```
   .use-absolute{
       position: relative;
       width:200px;
       height:150px;
       border:1px solid #000;
   }
   .use-absolute div{
       position: absolute;
       width:100px;
       height:50px;
       top:0;
       right:0;
       bottom:0;
       left:0;
       margin:auto;
       background:#f60;
   }
   ```

4. calc动态计算

5. js

   ```js
   .body {
       position: relative;
   }
   
   let HTML = document.documentElement,
       winW = HTML.clientWidth,
       winH = HTML.clientHeight,
       boxW = box.offsetWidth,
       boxH = box.offsetHeight;
   box.style.position = "absolute";
   box.style.left = (winW-boxW)/2 + 'px';
   box.style.top = (winH-boxH)/2 + 'px';
   
       
       
   ```

   
#### 10.p 的颜色？

 如果`class="classA classB"` 与`class="classB classA"` 相等吗？

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .classA{
            color:blue
        }
        .classB{
            color:red
        }
    </style>
</head>
<body>
    <p class="classA classB">ssssss</p>
</body>
</html>
```

#### 11.margin 塌陷

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width:100px;
            height: 100px;
            margin:10px;
            background-color: #f0f;
        }
        .box2{
            width:100px;
            height:100px;
            margin:20px;
            background-color: #0ff;
        }
    </style>
    
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
</body>
</html>
```

#### 12.1rem、1em、1vh、1px各自代表的含义？（media）

> rem

rem是全部的长度都相对于根元素<html>元素。通常做法是给html元素设置一个字体大小，然后其他元素的长度单位就为rem。

> em

- 子元素字体大小的em是相对于父元素字体大小
- 元素的width/height/padding/margin用em的话是相对于该元素的font-size

> vw/vh

全称是 Viewport Width 和 Viewport Height，视窗的宽度和高度，相当于 屏幕宽度和高度的 1%，不过，处理宽度的时候%单位更合适，处理高度的 话 vh 单位更好。

> px

px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。

一般电脑的分辨率有{1920*1024}等不同的分辨率

1920*1024 前者是屏幕宽度总共有1920个像素,后者则是高度为1024个像素

#### 13.介绍下BFC及其应用

BFC 就是块级格式上下文，是页面盒模型布局中的一种 CSS 渲染模式，相当于 一个独立的容器，里面的元素和外部的元素相互不影响。

创建BFC 的方式有：

1. html根元素
2. float浮动
3. 绝对定位
4. overflow 除了 visible 以外的值 (hidden、auto、scroll)

BFC 的主要作用是：

1. 清楚浮动
2. 防止同一BFC容器中的相邻元素间的外边距重叠问题
3. 可以利用BFC实现多栏布局

#### 14.css3吸顶效果

position:sticky

```css
<div class="header">

</div>
<nav>
    用于显示粘性定位的头
</nav>
<div class="content">

</div>
<footer>
    底部
</footer>


.header {
	width:100%;
	height:160px;
	background:#87CEEB;
}
nav {
	width:100%;
	height:100px;
	position:sticky;
	top:0px;
	background:#F98202;
}
.content {
	width:100%;
	background:blue;
	height:1000px;
}
footer {
	background:#87CEEB;
}

```

#### 15.圣杯布局和双飞翼布局

* flex
* 定位
* 浮动

#### 16.transition过度动画

* ```
  transition： CSS属性，花费时间，效果曲线(默认ease)，延迟时间(默认0)
  ```



* ```
  transition-property: width;
  transition-duration: 1s;
  transition-timing-function: linear;
  transition-delay: 2s;
  ```

#### 17.animation动画

* ```
  animation：动画名称，一个周期花费时间，运动曲线（默认ease），动画延迟（默认0），播放次数（默认1），是否反向播放动画（默认normal），是否暂停动画（默认running）
  ```

#### 18.img中alt和title的区别

* 图片中的 alt属性是在图片不能正常显示时出现的文本提示。alt有利于SEO优化
* 图片中的 title属性是在鼠标在移动到元素上的文本提示。

#### 19.用纯CSS创建一个三角形

* ```javascript
   <style>
      div {
          width: 0;
          height: 0;
          border-top: 40px solid transparent;
          border-left: 40px solid transparent;
          border-right: 40px solid transparent;
          border-bottom: 40px solid #ff0000;
      }
      </style>
  </head>
  <body>
    <div></div>
  </body>
  ```

  

#### 20.媒体查询

* ```
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="utf-8"> 
  <title></title> 
  <style>
  body {
      background-color: pink;
  }
  @media screen and (max-width: 960px) {
      body {
          background-color: darkgoldenrod;
      }
  }
  @media screen and (max-width: 480px) {
      body {
          background-color: lightgreen;
      }
  }
  </style>
  </head>
  <body>
  
  <h1>重置浏览器窗口查看效果！</h1>
  <p>如果媒体类型屏幕的可视窗口宽度小于 960 px ，背景颜色将改变。</p>
  <p>如果媒体类型屏幕的可视窗口宽度小于 480 px ，背景颜色将改变。</p>
  
  </body>
  </html>
  ```

  

#### 21.万能清除法 after伪类 清浮动（现在主流方法，推荐使用）

* ```
  float_div:after{
  content:".";
  clear:both;
  display:block;
  height:0;
  overflow:hidden;
  visibility:hidden;
  }
  .float_div{
  zoom:1
  }
  ```

  

#### 22.display:none 和 visibility: hidden的区别

* link属于HTML标签，而@import是CSS提供的页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载
* import只在IE5以上才能识别，而link是HTML标签，无兼容问题
* link方式的样式的权重 高于@import的权重.

#### 23.position的absolute与fixed共同点与不同点

* 共同点： 改变行内元素的呈现方式，display被置为block 让元素脱离普通流，不占据空间 默认会覆盖到非定位元素上
* 不同点： absolute的”根元素“是可以设置的 fixed的”根元素“固定为浏览器窗口。当你滚动网页，fixed元素与浏览器窗口之间的距离是不变的。

#### 24.transition和animation的区别

* Animation和transition大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是transition需要触发一个事件才能改变属性， 而animation不需要触发任何事件的情况下才会随时间改变属性值，并且transition为2帧，从from .... to，而animation可以一帧一帧的。
* transition 规定动画的名字 规定完成过渡效果需要多少秒或毫秒 规定速度效果 定义过渡效果何时开始 animation 指定要绑定到选择器的关键帧的名称.

#### 25.CSS优先级

* 不同级别：总结排序：!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性 1.属性后面加!import 会覆盖页面内任何位置定义的元素样式 2.作为style属性写在元素内的样式 3.id选择器 4.类选择器 5.标签选择器 6.通配符选择器（*） 7.浏览器自定义或继承 **同一级别：后写的会覆盖先写的*
* css选择器的解析原则：选择器定位DOM元素是从右往左的方向，这样可以尽早的过滤掉一些不必要的样式规则和元素

#### 26.CSS性能优化

* 1. 合并css文件，如果页面加载10个css文件,每个文件1k，那么也要比只加载一个100k的css文件慢。 
  2. 减少css嵌套，最好不要嵌套三层以上。 
  3.  不要在ID选择器前面进行嵌套，ID本来就是唯一的而且权限值大，嵌套完全是浪费性能。 
  4.  建立公共样式类，把相同样式提取出来作为公共类使用。
  5.  减少通配符*或者类似[hidden="true"]这类选择器的使用，挨个查找所有...这性能能好吗？ 
  6. 巧妙运用css的继承机制，如果父节点定义了，子节点就无需定义。 
  7. 拆分出公共css文件，对于比较大的项目可以将大部分页面的公共结构样式提取出来放到单独css文件里，这样一次下载 后就放到缓存里，当然这种做法会增加请求，具体做法应以实际情况而定。 
  8. 不用css表达式，表达式只是让你的代码显得更加酷炫，但是对性能的浪费可能是超乎你想象的。 
  9.  少用css rest，可能会觉得重置样式是规范，但是其实其中有很多操作是不必要不友好的，有需求有兴趣，可以选择normolize.css。 
  10.  cssSprite，合成所有icon图片，用宽高加上background-position的背景图方式显现icon图，这样很实用，减少了http请求。 11. 善后工作，css压缩(在线压缩工具 YUI Compressor) 12. GZIP压缩，是一种流行的文件压缩算法。

* ## 性能优化

  * ***\*避免使用@import，外部的css文件中使用@import会使得页面在加载时增加额外的延迟。\****
  * ***\*2.避免过分重排\****
  * ***\*repaint\****
  * **CSS动画**
  * **文件压缩**
  * **去除无用CSS**
  * **有选择地使用选择器**









