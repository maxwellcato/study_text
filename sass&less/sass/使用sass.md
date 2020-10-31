### sass的编译
##### 编译的命令行命令
 ```shell
  # 在终端中的格式 
  sass 'sass文件名':'css文件名'
  # 举例
  sass sass/sass.scss:css/style.css
  # 一般都是同一目录下创建sass文件夹和css文件夹，用来存放各自的代码
 ```

##### sass自带的自动编译sass

  ```shell
  # 格式:
  sass --watch sass:css  #冒号前后的sass和css均指的文件名，默认情况下，生成到css文件文件中的css类型的文件名和编译的sass文件名称一致。
  ```

##### sass编译的css文件的四种格式

  ```shell
  nested      嵌套   默认的格式，子元素会相对父元素缩进两格，且花括号的}不会在新的一行（我有点喜欢这种格式，父子关系看着比较直观）
  compact     紧凑   前边不留空格，一个元素占一行
  expanded    扩展   就以前手写css的那种格式
  compressed  压缩   前不留空格，后不换行的那种
  
  #修改编译风格的命令:
  sass sass/style.scss:css/style.css --style compressed  
  # 注：改style必须注明是哪个sass文件编译到哪个css文件，只写sass --style expanded没有效果
  # 或者写sass sass:css --watch --style compressed也是可以的，总之，用--style改变样式要有执行sass转换css的命令
  
      # 在这里遇到的问题，根据官网推荐的vscode上的一个live Sass Compiler插件，这个插件可通过实时浏览器重新加载来帮助您实时将SASS / SCSS文件编译/转换为CSS文件，我当时只是安装启用，没有做设置。然后在终端中修改编译风格时出现了问题，只有expanded和compressed是可以切换的，另外两个一切换就报错。而且按说，nested这种嵌套的才是默认样式，但是我是在安装完这个插件才开始创建sass文件并sass的，当时的样式已经变成expanded了。最后把这个插件禁用了才解决问题，感觉原因应该是这个插件精简了编译格式，只支持后边两种，且把第三种作为了默认样式
  ```

##### .sass和.scss的区别

  ```shell
  # .sass是sass最开始用的语法，后来开始支持.scss语法，后一种语法阅读性更强一些，两者区别其实蛮大的，而.scss更常用一些，也更新，之后的笔记也都是记.scss文件的用法，.sass的文件以后需要用到的时候再看吧
  ```

##### 变量   Variables

  ```scss
  $color: #aaa;
  $border: 1px solid $color;
  1.变量前必须要加$符号，引用的时候也要带上$符号
  2.变量也可以引用变量，最后都会被换成变量被定义的值。
//注意，变量分为全局变量和局部变量，在大括号内定义的变量即为局部变量，外部是无法引用的。另外，如果在局部变量后面加上!global，那个这个局部变量就将变成全局变量。

//变量操作
	1.== !=   支持所有的类型
	2.< > <= >=  仅仅支持数字类型
	3.+ - * / % 
  ```

##### 多值变量

```scss
//多值变量在定义上与单值变量相同
$paddings: 5px 10px 3px 20px;
//使用的时候，如果全用的话，也与单值变量相同
//如果只用其中一部分，则：
padding-top: nth($paddings,1)  //表示用其中的第一个数字

//在sass中，_和-是一样的。也就是说，变量$text-info和$text_info是一个变量
```



##### 嵌套Nesting

  ```scss
  .nav {
  	height:100px;
      ul {
          margin: 0;
          li {
              float: left;
              list-style:none;
              padding: 5px;
          }
      }
  }
  //等同于
  .nav {
  	height: 100px;
  }
  .nav ul {
      margin: 0;
  }
  .nav ul li {
  	float: left;
      list-sytle: none;
      padding: 5px;
  }
  ```

##### 嵌套时调用父选择器

  ```scss
  .nav {
  	height: 100px;
      a {
          color: blue;
          &:hover {
              color: green;
          }
      }
      & &-text {
          font-size: 15px;
      }
  }
  //将变成
  .nav{
      height:100px;
  }
  .nav a {
      color: blue;
  }
  .nav a:hover {
  	color: green;
  }
  .nav .nav-text {
      font-size: 15px;
  }
  //&符号在嵌套中直接代表父类元素的名称
  ```

##### 嵌套属性

  ```scss
  body {
  	font: {  //注意，这里要加冒号
  		size: 15px;
  		weight: normal;
  	}
  }
  .nav {
      border: 1px solid red {   //注意，这里没有冒号 ，也没有分号
          left: 0;
          right: 0;
      }
  }
  //将被转换成
  body {
      font-size: 15px;
      font-weight: normal;
  }
  .nav {
      border: 1px solid red;
      border-left: 0;
      border-right: 0;
  }
  ```

##### 跳出嵌套

```scss
@at-root //该指令只会跳出选择器嵌套，而不能跳出@media或@support,如果要跳出这两种，则需要使用@at-root(without:media),@at-root(without:support)。这个语法的关键词有四个：all(表示所有)、rule(表示常规css的常规选择器)、media(表示media)、support(表示support，因为@support目前还无法广泛使用，所以在此不表示)，我们默认的@at-root其实是@at-root(without:rule)
//用法
.text-info{
    color:red;
    @at-root nav &{
        color: blue;
    }
}
//将被编译成
.text-info{
    color: red;
}
nav .text-info {
	color: blue;
}
```



##### 混合Mixins

  ```scss
  @mixin 名字 (参数1，参数2...){
  	...
  }
  //举例:
  @mixin alert {
    color: white;
    background-color: red;
    a {
      color: blue;
    }
  }
  .alert {
    @include alert;  //不知道为啥，这里写成@include alert()也可以，然后分号两种情况都是可有可无
  }
  //效果
  .alert {
    color: white;
    background-color: red; }
    .alert a {
      color: blue; }
  ```

##### Mixin中的参数

```scss
@mixin alert($text-color,$background) {
  color: $text-color;
  background-color: $background;
  a {
    color: darken($text-color,30%); //darken(),将颜色加深多少，第一个参数跟颜色，第二个跟加深程度
  }
}
.alert {
  @include alert(white,red)
}
//编译成
.alert {
  color: white;
  background-color: red; }
  .alert a {
    color: #b3b3b3; }

//另外，参数如果在用的时候写上参数名，可以不按顺序写，下面这个效果跟上面的一样
@mixin alert($text-color,$background) {
  color: $text-color;
  background-color: $background;
  a {
    color: darken($text-color,30%);
  }
}
.alert {
  @include alert($background: red,$text-color:white)
}

//另外，如果混合只需一个参数，但是这个参数是个列表时
如：
@mixin box-shadow($shadow...){//注意，因为只是一个参数，而传值时会被误认为是两个参数，会报错，所以后边加...
  -moz-box-shadow: $shadow;
  -webkit-box-shadow: $shadow;
  box-shadow: $shadow; 
}

.shadow {
  @include box-shadow(0px 4px 4px #555,2px 6px 10px #3489fa)
}

//针对响应式布局
@mixin style-for-iphone{
  @media only screen and (min-device-width:320px) and (max-device-width:568px) {
    @content;
  }
}
@include style-for-iphone {
  font-size: 12px;
}
	//编译为
@media only screen and (min-device-width: 320px) and (max-device-width: 568px) {
  font-size: 12px;  //这里这个12px前边的冒号老是在vscode中报错，我不知道是为啥，找到原因了，说是因为在媒体布局中没有写元素：导致字体大小不知道是给谁设置的，在font-size外面套一个body就好了
}

```

##### 继承/扩展inheritance(让一个选择器继承另一个选择器的所有内容)

```scss
.alert {
  padding: 15px;
}
.alert a {
  font-weight: bold;
}
.alert-info {
  @extend .alert;
  background-color: #d9edf7;
}
//将被转换为
.alert, .alert-info {
  padding: 15px; }

.alert a, .alert-info a {
  font-weight: bold; }

.alert-info {
  background-color: #d9edf7; }
//不仅会继承元素本身的所有样式，还会继承子元素以及子元素的样式(问题？假如关于子元素的样式是在继承之后定义的，那么还会继承吗？)

//注意:练习注释的时候发现的，在把.alert注释掉的情况下，对.alert使用extend继承，
//还是会生成.alert-info a这个元素。
.alert a {
  font-weight: bolder;
}
.alert-info {
  @extend .alert;
  background-color: #d9edf7;
}
//转化
.alert a, .alert-info a {
  font-weight: bolder; }

.alert-info {
  background-color: #d9edf7; }

//多个继承的写法
可以写
@extend .alert;
@extend .small;
也可以
@extend .alert,.small
    
//占位选择器%
优势在于如果不调用则不会有任何多余的css文件，避免了以前在一些基础的文件中预定义了很多基础的样式，然后实际应用中不管是否使用了@extend去继承相应的样式，都会解析出来所有的样式。占位选择器以%标识定义，通过@extend继承
	//举例  .alert并不需要生效，只是作为一个基础样式方便被其他选择器继承而已，所以
%alert{
	background-color: red;        
}
.alert-info {
	@extend %alert;
    color: red;
}
	//这样，alert就不会出现在css文件中了
```

##### 继承的局限性

```scss
//虽然能继承的数量很多，但也有很多是不被支持的。如包含选择器(.one .two)或者是相邻兄弟选择器(.one+.two)
//另外如果继承的元素内部有hover属性，那么hover属性也会被继承。

//另外，好像继承也会把继承的东西的父元素也给继承了？试了好几次，确实是这样
footer {
  .box {
    width: 100px;
  }
}
.boxs {
  @extend .box;
}
 会被编译成
footer .box, footer .boxs {
  width: 100px;
}

//交叉继承(看着编译结果很奇怪)
a span {
  font-weight: bold;
}
div .content {
  @extend span
}
 会被编译成
a span, a div .content, div a .content {
  font-weight: bold;
}

//继承无法在使用@media之外的选择器继承的。
```



##### Partials和@import

```scss
//导入其他的css文件时，每使用一次都会使得服务器发送一次http请求，这样会加剧浏览器负担，所以，sass扩展了@import的功能，可以一次请求多个文件。这样我们在做项目的时候，可以把项目分成好多个scss文件，使用的时候选择性导入，每一个小文件被称作partials，每一个partials文件名要用下划线开头，这样，这些小文件不会被当做sass文件，编译成css文件

//创建一个_base.scss文件，在--watch模式下，对文件进行编辑后保存也不会在css文件中产生对应的css文件，然后引入的时候要这么用
@import "base";		//这样就把_base.scss中的内容编译进来了
```

##### 三种注释写法

```scss
//单行注释    均不保留
/*
*多行      会在除压缩形式外的其他三种编译方式下保留
*注释
*/

/*!
*强制     均保留，且在压缩模式下，依然保留有换行和空格
*注释
*/
```

##### 数字

```scss
//使用sass -i命令可以进入sass的交互模式

// scss中的字符串可用+连接，数字也可以通过加减乘除求余以及提供的一些函数进行四则运算
type-of(5)      // => "number"
type-of(5px)    // => "number"
type-of(hello)  // => "string"
type-of(1px solid red)  //=>"list"
type-of(5px 6px)   // => "list"
type-of(red)    // => "color"

abs()  //求绝对值
round()  //四舍五入
ceil()  //全入
floor()  //全舍
percentage(650px / 1000px)  // => 65%    百分比形式表示结果
min(1,2,3)   //返回最小值
max(1,2,3)   //返回最大值
```

##### 字符串

```scss
带引号不带引号(非颜色等关键字)都会被当做字符串，不过带了引号，可以加空格，而且'red'这种也会被识别成string，而不带引号直接写red的话会被当做color

// 使用 + 连接字符串
//处理字符串的函数
$greeting: "hello" //定义
$greeting   //回车会输出"hello"
to-upper-case()  //转换为大写
to-lower-case()  //小写
str-length()  //返回字符串长度
str-index($greeting,'he')  // => 1   返回子串的位置，注意索引是从1开始的，如果没有查询到，则返回null
str-insert($greeting,'.net',6) //将字符串插入到第一个位置的字符串的第6个位置，而且这个插入并不会改变$greeting的值，只是返回值被插入了。而且，第三个位置只要是大于6的，结果都跟6是一样的。然后写1和写0是一样的，但是当写-1时，是插入到最后，写-2时，插入到倒数第二的位置，然后依次类推，注意，当写负数表示从后往前数插入，当插入到最开始的位置时，数值再变小插入位置就不会再变了。
```

##### 颜色

```scss
//其他都跟css一样，然后有一个rgb(100%,0,0)的表示方法，跟rgb(255,0,0)一样，可能css也有？之前没用过
HSL(色相，饱和度，明度)
hsla(色相，饱和度，明度，透明度)

函数
adjust-hue(要调整的颜色(支持多种颜色表示方法)，要调整的度数(可为负))
adjust-hue(hsl(0,100,50%),137deg)
adjust-hue(#ff000,137deg) // => #00ff48

//以下几个函数用hsl表示颜色用起来效果最直观，因为亮度、饱和度都是hsl里的东西，当然其他表示方法也是可以用的
lighten和darken
(要调整的颜色，百分数)  //这个百分数不是在原有基础上增加多少，而是...比如原来的亮度是50%，我们写darken(颜色，30%)，那么最终的效果就是80%

saturate  //增加饱和度，或者说是纯度
desaturate  //减少饱和度，或者说是纯度

transparentize  //更透明 (要调整的颜色，要增加的透明度)   (rgba(255,0,0,0.5),0.3)
opacity  //更不透明 同上
```

##### 列表函数

```scss
scss中的列表有点类似于js中的数组
length()//输出列表长度  length(5px 10px)  => 2
nth()//得到列表里对应序号的项目 nth(5px 10px,1) => 5px
index //输出列表中出现这一项的序号

//向列表中添加内容
append(要插入新项目的列表，新项目，分隔符(不写默认空格，逗号的话要写'comma'))
append(5px 10px,3px 4px) // => (5px 10px (3px 4px))   新项目如果也是列表的话，不会被拆分，而是作为一个整体插进去
//两个列表合并  join(列表，列表，分隔符)
```

##### Map

```scss
//这是个数据类型，定义方法：
$map类型名: (key1:value1,key2:value2,key3:value3)
//用法
$color:(light:#fff,dart:#000)
map-get($color,dark) // => #000
map-keys($color)  // => ("light","dark")
map-values($color) // => (#fff,#000)
map-has-key($color,light)  // => true
map-merge(map类型,map类型)  //两个map合并
map-remove($color,light)  //删除color中的light项
```

##### 布尔值

```scss
大于小于等于表达式的结果为布尔值
and相当于&& ，or相当于||，not相当于!

//sass中还有null类型，这个类型就只有一个null
```

##### 样式/变量名中使用变量Interpolation

```scss
//在样式或者属性名上使用变量会报错(属性值使用不会报错)，所以，使用变量的时候，要这么写
#{变量名}
//在注释中使用#{变量名}也可以替换成变量的值
```

##### 控制指令

```shell
#if
@if $theme == dark {...}@else if $theme == light{...} @else{...}
	//还有一种不加@符号的if，用法如下：
	$screenWidth: 800;
    body{
        color: if($srceenWidth > 768,blue,red)  //三目运算符
    }

#for
$columns: 4
@for $i from 1 through $columns {  #如果把through换成to的话，只循环到3，不会循环到4
	.col-#{$i} {
		width: 100% / $columns * $i;
	}
}

#each
1.# 常规遍历
    $icons: success error warning;
    @each $icon in $icons {
        .icon-#{icon} {
            background-image: url(../images/icons/#{icon}.png)
        }
    }
2.# 遍历list
    @each $key,$value in (default,blue),(info,green),(danger,red){
        .text-#{$key}{
            color: $value;
        }
    }
	# 编译后：
    .text-default {
      color: blue;
    }
    .text-info {
      color: green;
    }
    .text-danger {
      color: red;
    }
3.# 遍历map
    @each $key,$value in (default:blue,info:green,danger: red){  # 跟2写法很接近
      .label-#{$key}{
        color: $value
        ;
      }
    }
    #编译后：
    .label-default {
      color: blue;
    }
    .label-info {
      color: green;
    }
    .label-danger {
      color: red;
    }

#while(while跟for的区别是，while可以设置步长)
$i: 6;
@while $i > 0 {
    .item-#{$i} {
        width: 5px * $i;
    }
    $i: $i-2  #注意，-号前后要留空格，不然会被当做横杠，试了下加号，加号不需要，不过为了统一，也为了读起来更直观，以后四则运算符前后都加空格吧
}
```

##### 用户自定义的函数

```scss
@function 名称 (参数1，参数2...) {
	...
}
//在自定义函数中，可以添加警告、错误信息，
例如:
$colors:(light: #fff,dark: #000);
@function color($key) {
    @if not map-has-key($colors,$key) {
        @error "在 $colors 中没找到 #{$key} 这个 key";  //错误信息会在终端中提示，而不是css中，如果是使用compass启动的编译，那么自定义的报错信息将在css中出现，也会在终端中提示(提示信息会被compass稍微包装一下)
    }
    @return map-get($colors,$key);
}
```

### 第二个视频(补充一下sass的知识)

##### sass和compass

```shell
# sass的.sass和.scss的区别除了语法的不同外，.scss还能对.css文件保持了兼容。

# compass是sass的工具库，类似于jQ对于js的关系。

安装compass命令
gem install compass就ok了。
gem查看源：gem resource
gem sources -r 移除源
gem sources -a http://...添加源
gem sources -u 更新源
# 这两次安装sass和compass都不要换源就能安装
```

+ 使用compass创建一个sass工程

  ```scss
  compass create style  //将会创建一个style的文件，其中有一系列的文件
  同时支持自定义创建文件的目录结构的自定义命令，这里暂不记录，详情请查看文档
  
  compass compile将工程文件中的scss文件编译到css文件夹中，不需要输入路径等参数，这个命令没有自动监视.scss更改保存后然后更新到.css中的功能。
  compass watch   实现自动更新更改，只改相对于之前有修改的.scss文件,如果要重新编译所有.scss文件，则需要命令compass watch --force
  
  //使用compass创建的工程文件中的配置文件是config.rb,在里面可以配置compass编译的一些参数
  ```

+ 使用compass更改编译风格

  ```shell
  # 加上--output-sytle STYLE
  # 简化的命令是-s STYLE
  # STYLE表示那四种风格
    nested      嵌套
    compact     紧凑
    expanded    扩展
    compressed  压缩
    
    compass --output-style STYLE
  ```

##### 自动化构建

+ gulp构建工具

  ```shell
  # 查看npm源命令
  npm config get registry
  # 更换npm源命令
  npm config set registry '地址'  # 默认的是https的，只要改成http，后面不变，就可以了
  ```

+ gulp-compass

+ browser-sync

+ Css3:linear-gradient()

+ 链接：https://www.bilibili.com/video/BV1KE411b7RQ?p=36

##### 啥

+ reset
+ normalize
+ susy
+ breakpoint
+ 页面Layout