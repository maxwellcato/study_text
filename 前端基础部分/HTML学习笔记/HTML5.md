### 新特性

```shell
# 新的语义元素(八个)，比如 <header>, <footer>, <article>, <section>,<nav>,<main>,<figure>,<aside>。这些东西似乎本质上都是div
# 新的表单控件，比如数字、日期、时间、日历和滑块。
# 强大的图像支持（借由 <canvas> 和 <svg>）
# 强大的多媒体支持（借由 <video> 和 <audio>）
# 强大的新 API，比如用本地存储取代 cookie。
```

### HTML5新元素

#### 自定义新元素（ie8及之前版本，不支持，要加些东西）

```html
...
<style>
  myHero {
    display: block;
    background-color: #ddd;
    padding: 50px;
    font-size: 30px;
  } 
  </style> 
...
<body>
    <h1>My First Heading</h1>
    <p>My first paragraph.</p>
    <myHero>My First Hero</myHero>
</body>

<!--标题所说的要加的东西>：

<!--[if lt IE 9]>
  <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```

#### video、source标签

```html

<video controls="controls">   <!-- controls为必加项，是用来控制文件播放暂停等的，另外，如果资源链接要用source标签来处理的话，则这里不能写一个空的src=""，不然下边的source标签不会被解析。另外，video需要定义宽高(不然是默认宽高) -->
    <source src="https://www.w3school.com.cn/i/movie.ogg" type="video/ogg">  <!-- 这两个source，说实话，随便注释掉一个都可以正常运行...，而且这俩不写的话，直接在video里边写src也是可以的 -->
    <source src="https://www.w3school.com.cn/i/movie.mp4" type="video/mp4">
</video>
```

#### article、aside等标签

```shell
# 这些标签本质上似乎是div标签？是div标签语义化的结果？，反正我用这些标签写个简单的样式，然后全部换成div后，页面布局什么的并没有发生改变
```

##### figure标签

```html
<fuck>没有定义的元素</fuck>
<fuck>没有定义的元素</fuck>
<fuck>没有定义的元素</fuck>
<figure>
  <p>黄浦江上的的卢浦大桥</p>
  <p>拍摄者：W3School 项目组，拍摄时间：2010 年 10 月</p>
  <img src="/i/shanghai_lupu_bridge.jpg" width="350" height="234" />
</figure>

<!-- figure的效果：
	1.首先，作为一个整体，会带有这些样式：
		display: block;
		margin-block-start: 1em;
		margin-block-end: 1em;
		margin-inline-start: 40px;
		margin-inline-end: 40px;
	2.没了，是我想错了。就这一个作用
	3.补充一个与figure配套的figcaption，这个元素一般用到img标签后面，相较于一般的行内标签，与图片距离更近，适用于书写图片标题
-->

<!-- 补充，我随便打了一个标签名，准备当做自定义标签的，没想到这标签竟然还真的有
	1.fieldset标签定义和用法：
        fieldset 元素可将表单内的相关元素分组。
        <fieldset> 标签将表单内容的一部分打包，生成一组相关表单的字段。
        当一组表单元素放到 <fieldset> 标签内时，浏览器会以特殊方式来显示它们，它们可能有特殊的边界、3D 效果，或者甚至可创建一个子表单来处理这些元素。
        <fieldset> 标签没有必需的或唯一的属性。
        <legend> 标签为 fieldset 元素定义标题。
-->
```

##### mark标签

```shell
# 像涂上荧光笔一样的效果，默认是黄色，被mark标签包起来的字体都会变成北京都会变成黄色

# 可以通过更改mark标签的背景颜色来达到更改默认颜色的效果(这个标签功能有点鸡肋，加span标签，然后修改其bgc，也可以达到同样的效果)
```

##### summary

```shell
# 上午才看过，下午就忘了...(好吧，是我记错了，那个是details)

# details标签是一个隐藏的文本，这个隐藏文本默认标题是‘隐藏的文本’，被ditails包裹的内容会在点击'隐藏的文本'之后显示出来
# 而修改默认的标题"隐藏的文本"的方法就是使用summary标签，该标签要被details包裹，然后其内的文本内容即为新的隐藏文本的标题
```



#### 表单元素

+ datalist（!important）

  ```html
  <!--效果超级好，建议全文背诵-->
  <input list="browsers">
  <datalist id="browsers">
      <option value="Internet Explorer">
      <option value="Firefox">
      <option value="Chrome">
      <option value="Opera">
      <option value="Safari">
  </datalist> 
  ```



### 新语法

```shell
# 使用input演示
<input type="text" value="John Doe" disabled>
<input type="text" value=John&nbsp;Doe Doe>
<input type="text" value="John Doe">
<input type="text" value='John Doe'>
# 最后三个效果完全一样，第一个无法进行编辑
```





## 非系统的学习遇到或学习到的碎片知识：

#### cvanvas的宽高设置问题

```shell
# 这个知识点说实话以前写那个画板demo的时候遇到过，不过已经好久没看这些东西了，遗忘情况有点严重。
# 废话不多说，问题就是如果canvas元素的宽高用css来设置的话，那么在画板上绘制的线条、图形会被莫名其妙的拉伸。canvas是由width和height属性的，最简单的用法就是直接把width 和height写到标签里(注意不是style的形式)，或者在js中给其width和hight赋值。如果是做全屏画板，获取到浏览器的宽高(使用resize监听浏览器变化，每次改变都更新画板的宽高，让其始终铺满浏览器)，然后赋值给画板。
```

