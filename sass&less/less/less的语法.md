##### 终端命令

```shell
# 学完scss再学less，感觉槽点满满，下边这个命令竟然不编译，只是在终端上把结果给我输出来了，终端输出有个毛用啊(less直接在文件中被引用蛮方便的)
lessc style.less  # 只在终端输出

lessc style.less style.css   # 这个会在同目录下将less文件编译到style.css文件中。

#压缩模式需要安装一个--clean-css插件，然后使用命令才能应用。且暂不能在浏览器引用中使用该插件，这东西好拉胯....
# 压缩编译命令(在已经安装了上面的插件的时候才能用，安装命令如下)
npm install -g less-plugin-clean-css
	# 编译编译命令：
lessc --clean-css style.less style.min.css
```

##### 在浏览器中使用less

```shell
# 在这里卡的时间有点长,视频上是这么用的，包括官网文档也是这么用的：
  <link rel="stylesheet/less" href="/sass_stydy/style.less">
  <script src="/sass_stydy/less.min.js"></script>
# 但是效果就是出不来，后来还用了cdn,官方给的cdn，还是不可以
# 查了半天，是需要使用vscode中的Live Server插件打开HTML文件，对插件做好了配置后发现，确实是成功了。原理尚不知道。本来以为使用less应该跟引用css一样很简单没啥限制的......之前的sass一直没有尝试在浏览器中引用，只是做了编译，可能到时候引用到浏览器中也需要不少的限制吧。
# 另外，如果在vue项目中使用的话，直接修改配置文件中的一项就可以了。
```

##### 变量

```shell
# 这个跟sass好像，sass是$color: red;这个是@color: red;

# 在属性名和选择器中使用变量的方法
@color: red;
@kuandu: width;
.@{kuandu} {
	@{kuandu}: 50px;
}

# 变量具有延迟加载，即变量在使用后再定义也是可以的。
# 一个变量重复定义时，这个情况稍微有点不好描述。即在当前作用域内，找最后面被定义的。找被定义的变量时，先找自己所在的大括号内的，没有的话再到外面找，自己所在大括号内的大括号是不会进去找的。这个蛮类似js的块级作用域的。
```

##### 混合mixin

```scss
1. //普通混合 把多个元素的共同的样式写到一个类(或者其他元素)中，然后把这个类写到多个元素中就可以了(注意，这样写的话，会在css中多输出一个.base的样式)
.base {
	color: white;
	border: 1px solid red;
}
header {
	font-size: 15px;
	.base;
}
footer {
	font-size: 20px;
	.base;
}
2. //不带输出的混合 把1里边的多输出的那个去掉的写法
.base() {
	color:white;
    border: 1px solid red;
}
3.//带选择器的混合   e,不好解释，看例子
.base() {
    &:hover {
        color: red;
    }
}
button {
    .base;
}
//输出：
button:hover {
    color: red;
}
4.//带参数的混合    看例子
.base(@color) {
    color:@color;
}
header {
    &:hover {
        .base(green);
    }
}
//然后，浮上去就绿了
5. //带参数，且有默认值(参数空着，使用默认值，写的话，按写的来)
.base(@color:red) {
    color:@color;
}
6.//带多个参数的混合  参数之间使用分号分割，有些时候可以用逗号，但是有些时候列表也是用逗号分割的，而这个会被当做是一个参数。所以，强烈建议使用分号分隔。
7.//多个具有相同名称和参数数量的混合(多个混合)   感觉这个没多大用......，视频里的例子是对mixin混合多次命名，想不明白这种写法有什么用意，而且看着输出结果跟less源码，也确实没找出来什么规律来，这个老师在这一点讲的也是乱七八糟的。
//额，看到一条弹幕突然懂了，就是引用mixin时，看你输入的参数值，如果输入的参数完全覆盖了定义的mixin的参数，那么这个mixin中的属性就会被引用，如果没有全面覆盖，但是没有被覆盖的都有默认值的话，那也是会被引用的。但是如果没有被覆盖的也没有默认值，那么这个定义的mixin里的所有属性都不会被引用。
//即根据输入的参数值，一个一个判断定义的混合的参数是否都有值，都有，就用这个混合里的内容，不符合，就不用
8.//命名参数 。跟sass中的使用混合时带参数赋值，这样可以不按顺序写
9.//arguments变量  同js函数中的arguments类似，代表所有可变的参数。需要注意的是，假如要给第一个和第三个赋值，不能写(值1，，值2),不能空，要写定义时候的默认值(其实也相当于赋值了)。这个是用在定义中的：
.base(@line:solid;@color:red){
    border: 1px @line @color;
}
//可以写成
.base(@line:solid;@color:red){
    border: 1px @arguments;
}
10.//匹配模式
.base(b_1,@line:solid;@color:red){...}
.base(b-2,@line:solid;@color:red){...}
//混合名一样，然后可以通过括号内第一个参数位置区分不同混合
11.//得到混合中变量的返回值
.base(@x;@y){
    @xy:(@x + @y)
}
//使用时
div {
    .base(10px;10px);
    margin:@xy;
}
```

##### 嵌套规则

```scss
//几乎跟sass一模一样
//不一样的地方(真的不一样吗？存疑，需要在sass里验证一下)：
1.
.a {
    .b {
        .c &{  //注意留空格，不然会变成.c.a .b {...}
            color:white;
        }
    }
}
  //会变成(孙子变爷爷)
.c .a .b {
    color: white;  
}
2.
a,b,c {
    font-size: 12px;
    & & {
        color: red;
    }
}
//然后会变成这样的，每个元素都要当每个元素的爸爸(包括自己)
a a,a b,a c,b a,b b,b c,c a,c b,c c {
    font-size: 12px;
}


```

##### 运算

```scss
//运算符两边必须留有空格

//老师讲了很多颜色的运算。。。。感觉完全用不到...
```

##### 函数

```shell
 1. # 首先，rgb()就是一种函数...???,那为啥css能支持,说他是不是都行吧
 2. # 提取蓝色值，blue()函数  提取rgb模式下的b的值，即后两位六进制的值，blue函数提取的是十进制的number类型
 3. # 
```

##### 命名空间

```scss
命名空间: // 将一些需要的混合组合在一起，可以通过嵌套多层id或者class来实现！
例:
//命名空间定义：
#bgcolor(){
	background-color: #ffffff;
    .a{
        color: #888888;
        &:hover{
            color:#ff6600
        }
        .b{
            background-color: #ff0000;
        }
    }
}
//使用：
.bgcolor1 {
    background-color:#fdfee0;
    #bgcolor>.a;
}
.bgcolor2 {
    #bgcolor>.a>.b
}
//等同于
.bgcolor1 {
    background-color:#fdfee0;
    .a{
        color: #888888;
        &:hover{
            color:#ff6600
        }
        .b{
            background-color: #ff0000;
        }
    }
}
.bgcolor2 {
    .b{
        background-color: #ff0000;
    }
}

//命名空间的选择使用跟子代、后代选择器有些像，都是使用>表示子选择，使用空格表示后代选择。
```

##### 作用域

```scss
// less中的作用域和函数类似，首先在自己所在的局部找变量定义，如果没有的话，往父部去找，依次类推。
```

##### 引入(import)

```scss
1.// 引入的写法：
@import "base";
2.//引入css文件的话，会被原样的引入，且编译也会原样编译到css文件中
3.//引入的css文件的内容，不能使用less的嵌套等规则，只能使用css的使用规则来用，不然会报错
4.// 可以带参数,用法：
@import (reference) "base.less";   //后缀好像是加不加都可以？
	once //默认，只包含一次
	reference , //没有输出.....只引用变量值，对于引用文件中定义的类等，不会在引用文件的文件中输出
    //举个例子
    @wt: 960px;
	.color{
        color: darkgreen;
	}
	//对以上内容做引用的话，使用@import (reference) "base.less"的方式去引用，那么只能使用其中的变量，.color的内容会被忽略掉，这就是所谓的没有输出。
    inline, //引用文件，但不进行操作，即变量不能用，引用文件定义的类啥的也不能嵌套用
    less //将文件作为less文件对象，不管原文件的后缀名是啥
	css  //将文件作为css文件对象，不管原文件的后缀名是啥
	multiple  //允许对同一文件引用多次(引入几次，里边的东西在文件里就出现几次(暂不清楚这个参数的应用场景是啥))
```

##### 关键字

```scss
// 在调用的混合集后面加入!important关键字，可以使混合集里面的所有属性都继承!important  不推荐使用
```

##### 条件表达式

+ 比较运算符

  ```
  > < >= <= true
  ```

+ 类型检查函数

  ```
  基本的：
  iscolor
  isnumber
  isstring
  iskeyword  //是否是关键字
  isurl
  ```

+ 单位检查函数

  ```
  ispixel 是否是px
  ispercentage   百分比
  isem   em
  isunit   单元
  ```

##### 循环(loop)

```scss
//在less中，混合可以调用自身，这样，当一个混合递归调用自己，再结合Guard表达式和模式匹配这两个特性，就可以写出循环结构。
示例：(这个.loop到底是关键字还是自定义的混合？自定义的!!!，用其他的单词也是可以的)
.loop(@counter)when(@counter>0){
    .loop((@counter - 1));  //递归调用自身
    width:(10px * @counter);
}
div{
    .loop(5);
}
//然后将依次输出：
10px
20px
30px
40px
50px
//这个好像是递归内部的先输出？又仔细看了一下，这个输出顺序很合理，没问题
```

##### 合并属性

```scss
1. //在需要合并的属性的:的前面加上+就可以完成合并,合并以,分割属性(属性在混入中的定义和属性的使用都要加+号)
.mixin(){
  box-shadow+: inset 0 0 10px #555;
}
.color {
  .mixin();
  box-shadow+: 0 0 20px black;
}
2. //以空格为分割符合并属性
.mixin(){
    box-shadow+_: inset 0 0 10px #555;
}
.color {
	.mixin();
    box-shadow+_:0 0 20px black;
}
```

##### 其他函数

```
color    将字符串转换成颜色(有啥用？)
convert   单位转换函数
data-url   将资源内嵌到样式文件
default   只能在边界条件中使用，没有匹配到其他自定义函数(mixin)的时候返回true，否则返回false
unit    移除或改变属性值的单位
```

##### 字符串函数

```
escape()   将输入字符串的url特殊字符进行编码处理
e()        原样输出函数内的内容  ，可用~符号代替
%()        格式化字符串  (内容有点多，查文档吧)
replace()  
replace('hello hi','hi','nihao')   =>   'hello nihao'
```

##### 长度相关函数

```
length()   返回集合中值的个数
extract()  返回集合中指定索引的值
```

##### 数学函数

```
ceil()
floor()
percentage()  转化为百分数
round()  四舍五入
sqrt()   计算平方根，单位保留
abs()    计算绝对值，单位保留
sin()
asin()
cos()
acos()
tan()
atan()
pi()    返回π
pow()    乘方运算符  pow(a,b)  a的b次方，a一般带单位，b不带
mod()    取余
min()
max()
```

##### 类型函数

```
isnumber()
isstring()
iscolor()
iskeyword()  判断是否是关键字
isurl()   判断是否是url地址
ispixel()
isem()
ispercentage()
isunit()   判断是否是指定单位的数据 ，第一个参数时数据，第二个是单位
```

##### 颜色值定义函数

```
rgb()
rgba()
argb()  //创建格式为#AARRGGBB的16进制的颜色值，ie滤镜、安卓等的开发中使用。(注意不是#RRGGBBAA)
hls()
hsla()   //色相、饱和度、亮度、透明度
hsv()    
hsva()   //色相、饱和度、色相、透明度
```

##### 颜色值提取函数

```
hue()   从HSL颜色值中提取色相值
saturation()    饱和度
lightness()    亮度
hsvhue()    从HSV颜色值中提取色相值
hsvsaturation()    饱和度
hsvvalue()    色调值
red()    rgb中的r值
green()    g值
blue()    b值
alpha()   透明度
luma()   计算颜色对象luma的值(亮度的百分比表示法)
luminance()    计算伽马校正的亮度值
```

##### 颜色值运算函数

```
saturate()   增加饱和度
desaturate()    降低饱和度
lighten()    增加亮度
darken()    降低亮度
fadein()   增加不透明度	
fadeout()    增加透明度
fade()    设定透明度
spin()    任意方向旋转颜色的色相角度
mix()    根据比例混合两种颜色，包括计算不透明度
greyscale()    完全移除颜色的饱和度
contrast()    传入多个颜色，哪个颜色的对比度大就用哪个
```

##### 颜色混合函数

```
multiply()
screen()
overlay()
softlight()
hardlinght()
difference()
exclusion()
average()
negation()
这些函数分别对应Photoshop中的一些图像颜色处理操作。我并没有看到什么应用场景。
讲解链接：https://www.bilibili.com/video/BV1KE411b7RQ?p=24
```

