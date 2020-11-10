### jquery事件

##### jquery事件注册

+ 单个事件注册

  ```shell
  element.事件(function(){}) 
  
  $(“div”).click(function(){  事件处理程序 })
  
  其他事件和原生基本一致。
  比如mouseover、mouseout、blur、focus、change、keydown、keyup、resize(这个好像只能用window来触发，绑定到其他元素上，即使尺寸变了也不会触发)、scroll(该元素只要有滚动条，就可以绑定该事件并触发) 等
  1.# 补充: 前台，绑定事件的是A，A里面还有子元素B，那么:
  	# A元素绑定mouseover和mouseout事件时，鼠标由A元素移动到B元素上，这两个事件也会被触发
  	# A元素绑定mouseenter和mouseleave事件时,鼠标从A移动到B上，不会被触发
  2.# 补充:keypress和keydown的区别
  	# 相同点:都是按着键盘不送，一直触发，这跟keyup不一样
  	# 不同点:
  		# 按键盘上的任意键都会触发keydown事件，而keypress只能在按字符键时触发(如果是拼音输入的话，按字符和空格都不会触发(keydown是全部触发)...)
  		# 因为上一条原因(按功能键不会触发)，keypress和(keydown keyup)的e.keyCode的值并不完全等同(数字键相同，字母键不相同)
  		# 二者的根本区别是, 系统由KeyDown返回键盘的代码, 然后由TranslateMessage函数翻译成成字符, 由KeyPress返回字符值. 因此在KeyDown中返回的是键盘的代码, 而KeyPress返回的是ASCII字符. 所以根据你的目的, 如果只想读取字符, 用KeyPress, 如果想读各键的状态, 用KeyDown.
  	# 按下键盘会触发键盘事件，顺序依次为：keydown->keypress->keyup.
  ```

##### 事件处理

+ 事件处理on()绑定事件

  ```js
  on() 方法在匹配元素上绑定一个或多个事件的事件处理函数:
  
  element.on(events,[selector],fn)
  
  1. events:一个或多个用空格分隔的事件类型，如"click"或"keydown" 。
  2. selector: 元素的子元素选择器 。
  3. fn:回调函数 即绑定在元素身上的侦听函数
  ```

  + **2.1** **事件处理** **on()** **绑定事件**

    ```shell
    1.# on() 方法优势1:
    	# 可以绑定多个事件，多个事件处理程序
             $(“div”).on({
                 mouseover: function(){}, 
                 mouseout: function(){},
                 click: function(){}	
             });
    	# 如果事件处理程序相同 :
            $(“div”).on(“mouseover mouseout”, function() {
                $(this).toggleClass(“current”);
            });       
    
    2.# on() 方法优势2:
    	#可以事件委派操作 。事件委派的定义就是，把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素:
            $('ul').on('click', 'li', function() {
                alert('hello world!');
            });       
        注: 在此之前有bind(), live() delegate()等方法来处理事件绑定或者事件委派，最新版本的请用on替代他们。
    3.# on() 方法优势3:
    	# 动态创建的元素，click() 没有办法绑定事件， on() 可以给动态生成的元素绑定事件 
            $(“div").on("click",”p”, function(){
                 alert("俺可以给动态生成的元素绑定事件")
             });       
    		
    		$("div").append($("<p>我是动态创建的p</p>"));
    		# 注意: 
    			# 如果不使用事件委托进行绑定的话，那么需要在创建元素之后进行绑定(这样的话，直接使用.click(...)也可以实现，并不需要.on也能实现)，而如果我们使用事件委托的方式进行绑定，则不需要在乎两者的顺序
    ```
  
+ **2.2** **事件处理** **off()** **解绑事件**
  
  + off() 方法可以移除通过 on() 方法添加的事件处理程序
  
      ```shell
          $("p").off() // 解绑p元素所有事件处理程序
      
          $("p").off( "click")  // 解绑p元素上面的点击事件 后面的 foo 是侦听函数名(课件上没写foo，自己写了一下，完全不知道咋实现了...)
          # 后面如果跟上函数名的话(绑定时候给函数定义的名字)，会比不加多触发一次函数(忘了当时是咋绑定的，一写绑定时定义的函数名，就报错啊)
      
        $("ul").off("click", "li");   // 解绑事件委托(有点怀疑这个‘li’有没有必要加上去，直接把click从ul上解绑掉就达到解除的效果了...,难道加上有性能优化的好处？)
    ```
  
  + 如果有的事件只想触发一次， 可以使用 one() 来绑定事件。
    
      ```js
      // 已实测，没有two()这个方法，不过如果想要绑定两次的话，one方法里再使用一次one就可以了
      $('.box').one('click',function(){
      	console.log('点我干啥')
          $('.box').one('click',function(){
              console.log('怎么又点')
          })
    })
    ```
  
+ **2.3** **自动触发事件 trigger()** 

  + 有些事件希望自动触发, 比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发
  
    ```js
    1.
    element.click()  // 第一种简写形式,先定义手动操作的事件,然后定义trigger自动事件
    
    2.
    element.trigger("type") // 第二种自动触发模式，也是先定义手动操作的事件，然后定义trigger自动事件
    //举例:
        $("p").on("click", function () {
          alert("hi~");
        }); 
    
        $("p").trigger("click"); // 此时自动触发点击事件，不需要鼠标点击，不过只会自动触发一次，如果要达到轮播图那种，点击后触发的话需要把这个事件绑定到轮播转换的按钮上，点击一次触发一次才能达到应有的效果。
    
    3.
    element.triggerHandler(type)  // 第三种自动触发模式
    // triggerHandler模式不会触发元素的默认行为，这是和前面两种的区别，不过用法和前两个是一样的，也要先给事件绑定到元素上，然后再绑定这个自动的东西
    ```
  
##### jquery事件对象

+ 事件被触发，就会有事件对象的产生

  ```js
  element.on(events,[selector],function(event) {})       
  
  阻止默认行为：event.preventDefault()   或者 return  false   //说实话，不知道这个是啥效果
  阻止冒泡： event.stopPropagation()      
  // 冒泡现象在这里的表现就是，把父元素和子元素各绑定一个事件(事件要相同，如点击事件)，点击子元素时，父元素绑定到点击事件上的函数也会被触发
  ```

  

  
  
### jquery常用API

##### jQuery选择器

+ 基础选择器

  ```js
  $(“选择器”)   //  里面选择器直接写 CSS 选择器即可，但是要加引号 
  
  ID选择器  $("#id")
  全选选择器   $("*")
  类选择器  $('.class')
  标签选择器  $('div')
  并集选择器  $('div,p,li')
  交集选择器  $('li.current')
  ```

+ 层级选择器

  ```js
  子代选择器  $('ul>li')
  后代选择器  $('ul li')
  ```

+ 隐式迭代

  ```js
  遍历内部 DOM 元素（伪数组形式存储）的过程就叫做隐式迭代
  
  简单理解：给匹配到的所有元素进行循环遍历，执行相应的方法，而不用我们再进行循环，简化我们的操作，方便我们调用。
  自己的理解:我们使用$()选择了好几个元素，直接在后面跟.事件(function(){})就能把这几个元素全部绑定上这些事件，这就是隐式迭代。
  ```

+ jquery筛选选择器

  ```js
  :first   获取第一个元素
  :last  获取最后一个元素
  :eq(index)  获取第index+1个元素，index从0开始的
  :odd  获取奇数排位的元素
  :even  获取偶数排位的元素
  ```

+ jquery筛选方法

  ```js
  parent()  查找父级
  childern()  查找子元素，相当于('ul>li')这种
  find()  查找后代
  siblings()  查找除自己外的所有兄弟元素
  nextAll()  当前元素之后的所有兄弟元素
  prevtAll()  当期元素之前的所有兄弟元素
  hasClass()  判断该元素是否含有某个特定的类
  eq(index)  相当于$(':eq(index)') 这种
  
  // 重点记住： parent()  children()  find()  siblings()  eq()
  
  ```

+ jquery里的排他思想

  ```
  想要多选一的效果，排他思想：当前元素设置样式，其余的兄弟元素清除样式
  
  $(this).css(“color”,”red”);
  $(this).siblings().css(“color”,” ”);
  ```

+ 链式编程

  ```js
  $(this).css('color', 'red').siblings().css('color', ''); 
  
  //这就是链式编程
  ```

##### jquery样式操作

+ 操作css方法

  ```js
  jQuery 可以使用 css 方法来修改简单元素样式； 也可以操作类，修改多个样式。
  1. 参数只写属性名，则是返回属性值
  	$(this).css("color");
  2.  参数是属性名，属性值，逗号分隔，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号
  	$(this).css("color", "red");
  3.  参数可以是对象形式，方便设置多组样式。属性名和属性值用冒号隔开， 属性可以不用加引号
  	$(this).css({ "color":"white","font-size":"20px"});
  ```

+ 设置类样式方法

  ```js
  // 作用等同于以前的 classList，可以操作类样式， 注意操作类里面的参数不要加点
  1.添加类
  	$(“div”).addClass(''current'');
  2.  移除类
  	$(“div”).removeClass(''current'');
  3.  切换类
  	$(“div”).toggleClass(''current'');
  ```

+ 类操作与className区别

  ```js
  原生 JS 中 className 会覆盖元素原先里面的类名。  //是的，已经验证过了
  jQuery 里面类操作只是对指定类进行操作，不影响原先的类名。
  
  //html5中新增的操作类名的方式 classList ，有4个属性
  
  add(value)：将给定的字符串值添加到列表中。如果值已经存在，就不添加了。
  contains(value)：表示列表中是否存在给定的值，如果存在则返回 true，否则返回 false。
  remove(value)：从列表中删除给定的字符串。
  toggle(value)：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它
  
  //关于className补充：
  如果一个元素的class有多个值，那么当我们输出这个元素的className的时候，将会输出一个由多个class组成中间由空格隔开的字符串，这也是对className赋值会覆盖掉原先里面的类名的原因
  ```

##### jquery效果

+ jquery封装的动画效果:

  ```shell
  # 显示隐藏
  	show()
  	hide()
  	toggle()
  # 滑动
  	slideDown()
  	slideUp()
  	slideToggle()
  # 淡入淡出
  	fadeIn()
  	fadeOut()
  	fadeToggle()
  	fadeTo()
  # 自定义动画
  	animate()
  ```

+ 显示隐藏效果

  ```js
  //显示语法规范:
  	show([speed,[easing],[fn]])  // 前两个参数都要加引号的
  //显示参数：
      （1）参数都可以省略， 无动画直接显示。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  
  //隐藏语法规范
      hide([speed,[easing],[fn]])
  //隐藏参数
      （1）参数都可以省略， 无动画直接显示。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  
  //隐藏显示语法规范
      toggle([speed,[easing],[fn]])
  //切换参数
  	（1）参数都可以省略， 无动画直接显示。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次
  
  //建议: 平时一般不带参数，直接显示隐藏即可
      
  //补充，上源码：
      $('button').on('click',function(){
          $('li').eq(1).stop().hide(5000,'linear',()=>{console.log('隐藏完成')})
          setTimeout(() => {
              $('li').eq(1).stop().show(5000,'linear',()=>{console.log('妖怪，显形')})
          }, 2000);
      })
  	//变形过程分析：前两秒，隐藏函数正常运行，两秒后，隐藏函数停止运行，开始运行显示函数，虽然显示函数运行不是从完全隐藏的时候运行的，但运行显示函数依然用了五秒。然后输出的话，只输出了show的回调函数，hide的回调函数因为没有完成操作所以并没有运行
  	//且变形过程是由右下角向左上角缩小，假如不加参数的话，则是瞬间完成。不过...还是上边的函数，假如隐藏的我们保留参数，把显现的参数全部去掉的话，那么最后元素会在两秒后开始显现时定格不动。如果我们把settimeout的时间定为5000的话，则因为异步的原因，函数实际会在5000毫秒多一点之后执行，所以定为5000函数难以显形(元素变的很小，视觉上是已经消失的，不过实际上还是在的，所以只是在很小的时候暂停住，不会显现成原来的大小)，定义成5010的话则可以完成显形
  	//用后台看了一下，用5000的话，元素大小从原来的320*52变成了0.几*0.几
  	//再次补充，每点击一次按钮都会运行先隐藏后显现的命令，如果把settimeout设置成4900，那么，每次都会花5秒的时间去缩小自己尺寸，速度就是当前大小到完全隐藏不见的速度，不过因为4900毫秒后会被停止，所以，按好几次按钮，这个元素也不会到小到消失(按了好几次，尺度已经到10的负21次方这个量级了，还是不会显现，不过由于settimeout的精度问题，我把时间调到隐藏100和settimeout99的时候，按几次会彻底把元素隐藏掉，从而运行显现函数)
  ```

+ 滑动效果

  ```js
  //下滑效果语法规范
  	slideDown([speed,[easing],[fn]])
  //下滑效果参数
      （1）参数都可以省略。
      （2）speed:三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing:(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  
  //上滑效果语法规范
      slideUp([speed,[easing],[fn]])
  //上滑效果参数
      （1）参数都可以省略。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  
  //滑动切换效果语法规范
      slideToggle([speed,[easing],[fn]])
  //滑动切换效果参数
      （1）参数都可以省略。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  ```

+ 事件切换

  ```js
  hover([over,]out)
  （1）over:鼠标移到元素上要触发的函数（相当于mouseenter）
  （2）out:鼠标移出元素要触发的函数（相当于mouseleave）
  （3）如果只写一个函数，则鼠标经过和离开都会触发它
  # 完全不懂，这啥意思呀？咋写的？啥作用啊？
  	//这个并不是一种的普适性语法规则，只是鼠标经过和离开的复合写法
  //这样用的：
  $('.box').hover(function(){},function(){})   //这个意思是，鼠标经过时触发第一个函数，离开时触发第二个函数
  	//.hover事件是鼠标经过和离开的复合写法，不支持on绑定事件方法
  ```

+ 动画队列及其停止排队方式

  ```js
  //动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行。
  
  stop()方法
  (1）stop() 方法用于停止动画或效果。
  (2)  注意： stop() 写到动画或者效果的前面， 相当于停止结束上一次的动画
  ```

+ 淡入淡出效果

  ```shell
  # 淡入效果语法规范
  	fadeIn([speed,[easing],[fn]])
  # 淡入效果参数
      （1）参数都可以省略。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次.
      
  # 淡出效果语法规范
  	fadeOut([speed,[easing],[fn]])
  # 淡出效果参数
  	（1）参数都可以省略。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。(这个省略的话，不影响后边写回调函数)
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
      
  # 淡入淡出切换效果语法规范
  	fadeToggle([speed,[easing],[fn]])
  # 淡入淡出切换效果参数
      （1）参数都可以省略。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
      
  # 渐进方式调整到指定的不透明度
  	fadeTo([[speed],opacity,[easing],[fn]])
  # 效果参数
      （1）opacity 透明度必须写，取值 0~1 之间。
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。必须写
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  ```

+ 自定义动画 animate

  ```shell
  # 语法
  animate(params,[speed],[easing],[fn])
  # 参数
      （1）params: 想要更改的样式属性，以对象形式传递，必须写。 属性名可以不用带引号， 如果是复合属性则需要采取驼峰命名法 borderLeft。其余参数都可以省略。(不知道为啥，一直没办法改变color的值,bgc颜色的值也没法改，然后使用border的话，在原来的基础上把1px变成2px也没法激活，而使用borderWidth则可以激活...好像只能改带有数值的值？)
      	# 补充：
      		# jquery中的animate方法的实现原理是用定时器根据步长来逐渐改变属性值，因此只支持数值型的属性改变，颜色不具有十进制的增减性，所以实现不了。不过，可以在jquery.ui库里面得以实现。再不过...为了一个颜色的变化而去引入一个jquery.ui这么一个大库的话，还不如自己写过度来的实在~~
      		
      		
      （2）speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
      （3）easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
      （4）fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  ```

##### jquery属性操作

+ 设置或获取元素固有属性prop()

  ```shell
  # 所谓元素固有属性就是元素本身自带的属性，比如 <a> 元素里面的 href ，比如 <input> 元素里面的 type(必须是在标签内有写的，input倒是不写也可以获取到，因为默认是text)
  
      # 获取属性语法
          prop(''属性'')
      # 设置属性语法
          prop(''属性'', ''属性值'')
  
  # 设置或获取元素自定义属性值 attr()   (用prop可以完成的事，用attr也是可以的，不过，如果input把type省略掉这种情况，使用prop可以获得text的默认属性值，而attr的话则会undefined，这样看来的话，attr()就是操作写到标签内的属性的，省略了就不行了)
  	用户自己给元素添加的属性，我们称为自定义属性。 比如给 div 添加 index =“1”
      # 获取属性语法
          attr(''属性'')      // 类似原生 getAttribute()
      # 设置属性语法
          attr(''属性'', ''属性值'')   // 类似原生 setAttribute()
          # 注：该方法也可以获取 H5 自定义属性
          
  # 数据缓存 data()
  	data() 方法可以在指定的元素上存取数据，且不会修改 DOM 元素结构。一旦页面刷新，之前存放的数据都将被移除
  	# 附加数据语法
  		data(''name'',''value'')   // 向被选元素附加数据   
  	# 获取数据语法
  		date(''name'')             //   向被选元素获取数据   
  	# 注：同时，还可以读取 HTML5 自定义属性  data-index ，得到的是数字型
  ```

##### jquery内容文本值

+ 普通元素内容 html()（ 相当于原生inner HTML)  标签也会被输出

  ```js
  html()             // 获取元素的内容
  
  html("内容")   // 设置元素的内容
  ```

+ 普通元素文本内容 text()  (相当与原生innerText）  只输出文本

  ```js
  text()                     // 获取元素的文本内容
  
  text("文本内容")   // 设置元素的文本内容
  ```

+ 表单的值 val()（ 相当于原生value)

  ```js
  val()              // 获取表单的值
  
  val("内容")   // 设置表单的值
  ```

##### jquery元素操作

+ 遍历对象

  ```js
  jQuery 隐式迭代是对同一类元素做了同样的操作。 如果想要给同一类元素做不同操作，就需要用到遍历。
  // 语法1:
      1. each() 方法遍历匹配的每一个元素。主要用DOM处理。 each 每一个
      2. 里面的回调函数有2个参数：  index 是每个元素的索引号;  demEle 是每个DOM元素对象，不是jquery对象
      3. 所以要想使用jquery方法，需要给这个dom元素转换为jquery对象  $(domEle)
  //语法2:
  $.each(object，function (index, element) { xxx; }) 
  	//这里边的object既可以是jquery对象，也可以是DOM元素对象
  
  1. $.each()方法可用于遍历任何对象。主要用于数据处理，比如数组，对象
  2. 里面的函数有2个参数:    index 是每个元素的索引号;  element  遍历内容
  ```

+ 创建元素

  ```js
  $("<li></li>")
  
  动态的创建了一个 <li>  
  //注意：
      这里如果要是写成<li><li>的话，会动态创建出来俩
  ```

+ 添加元素

  ```shell
  # 内部添加
      element.append(''内容'')  
      	把内容放入匹配元素内部最后面，类似原生 appendChild
      	
      element.prepend(''内容'')  
  		把内容放入匹配元素内部最前面。
  ```

+ 删除元素

  ```js
  element.remove()   //  删除匹配的元素（本身）
  element.empty()    //  删除匹配的元素集合中所有的子节点
  element.html("")   //  清空匹配的元素内容
  
  remove 删除元素本身。
  empt() 和  html('''') 作用等价，都可以删除元素里面的内容，只不过 html 还可以设置内容。
  //注意，删除元素的话,删除第一个元素，第二个会变成第一个，依次类推的这种。
  ```

##### jquery尺寸、位置操作

+ jquery尺寸

  ```js
  width()/height()    取得匹配元素宽度和高度值 ，只算width/height  //这里隐式迭代不会被触发
  innderWidth()/innerHeight()    取得匹配元素宽度和高度值，包含padding  //这里隐式迭代不会被触发
  outerWidth()/outerHeight()    取得匹配元素宽度和高度值，包含padding、border  //这里隐式迭代不会被触发
  outerWidth(true)/outerHeight(true)    取得匹配元素宽度和	高度值，包含padding、border、margin  //这里隐式迭代也不会被触发
  
  以上参数为空，则是获取相应值，返回的是数字型。
  如果参数为数字，则是修改相应值。  //注意，这里如果修改相应值的话，隐式迭代会被触发
  参数可以不必写单位。
  
  ```

+ jquery位置

  ```shell
  # 位置主要有三个： offset()、position()、scrollTop()/scrollLeft()
  
  1.offset()设置或获取元素偏移
      offset() 方法设置或返回被选元素相对于文档的偏移坐标，跟父级没有关系。
      该方法有2个属性 left、top 。offset().top  用于获取距离文档顶部的距离，offset().left 用于获取距离文档左侧的距离。
      可以设置元素的偏移：offset({ top: 10, left: 30 });
      
  2.position() 获取元素偏移
      position() 方法用于返回被选元素相对于带有定位的父级偏移坐标，如果父级都没有定位，则以文档为准。
      该方法有2个属性 left、top。position().top 用于获取距离定位父级顶部的距离，position().left 用于获取距离定位父级左侧的距离。
      该方法只能获取，不能设置 
     
  3.scrollTop()/scrollLeft() 设置或获取元素被卷去的头部和左侧
      scrollTop() 方法设置或返回被选元素被卷去的头部。
      不跟参数是获取，参数为不带单位的数字则是设置被卷去的头部
  ```

### 补充

##### readey函数

+ 当 DOM（文档对象模型） 已经加载，并且页面（包括图像）已经完全呈现时，会发生 ready 事件。

  由于该事件在文档就绪后发生，因此把所有其他的 jQuery 事件和函数置于该事件中是非常好的做法。正如上面的例子中那样。

  ready() 函数规定当 ready 事件发生时执行的代码。

  ready() 函数仅能用于当前文档，因此无需选择器。

+ ### 语法 1

  ```js
  $(document).ready(function)
  ```

  ### 语法 2

  ```js
  $().ready(function)
  ```

  ### 语法 3

  ```js
  $(function)
  ```

##### 属性选择器

+ 选取所有带有href属性的元素

  ```js
  $("[href]")
  ```

+ 选取所有带有href属性且等于"#"的元素

  ```js
  $("[href='#']")
  ```

+ 选取所有带有href且值不等于"#"的元素   (这个好像是找到所有非href值等于#的元素，没有href属性的也会被勾选上，所以用的时候，要在前面加上要加上有href值的选择)

  ```js
  $('[href][href!="#"]')   //这样就能选择有href但是href不等于"#"的元素了
  ```

+ 选取所有href值以'.jpg'结尾的元素

  ```js
  $('[href$='.jpg']')
  ```

##### 层级选择器补充

+ prev + next 匹配所有紧接在prev元素后的next元素

  ```js
  $('label+input')  匹配所有跟在lable后边的input元素(注意，就后面那一个元素)
  ```

+ prev ~ siblings 匹配所有紧接在prev元素后的所有siblings元素

  ```js
  $('form~input')  找到所有form表单同辈的input元素
  ```

##### 简单的选择器补充

+ :not(selector)  去除所有不跟给定选择器匹配的元素

  ```js
  $('input:not(:checked)')    查找所有未选中的input元素
  ```

+ :gt(index)  匹配所有大于给定索引值的元素

  ```js
  $('tr:gt(0)')   查找所有索引大于0的行
  ```

+ :lt(index)  匹配所有小于给定索引值的元素

+ :header    匹配所有标题元素

  ```js
  $(':header').css({"color","red"})  //把所有标题字体颜色设置为红色
  ```

+ :animated  匹配所有正在执行动画效果的元素

##### 内容选择器补充

+ :contains(text)    匹配包含给定文本的元素

  ```js
  $("div:contains('John')")    查找所有包含"John"的div元素
  $(':contains("点")').css('color','red')  查找所有包含"点"的元素将其字体颜色设置为红色#错误，如果不限制是什么元素的话，通过文本选择元素会很混乱
  ```

+ :empty  匹配不包含子元素或者文本的空元素

  ```js
  $("td:empty")  匹配没有子元素或空文本的td元素
  ```

+ :has(selector)  匹配含有选择器所匹配的元素的元素

  ```js
  $("div:has(p)").addClass('test')    给所有包含p元素的div元素添加一个样式
  ```

+ :parent  选取所有带有子元素或包含文本的<td>元素：

  ```js
  $("td:parent")    查找所有含有子元素	或者文本的td元素
  ```

##### 可见性选择器

+ :hidden   匹配所有的不可见元素(包含display=none和input=hidden的)   

  ```js
  # 有点不对劲啊，不知道为什么:hidden不加前缀直接写$(':hidden').css('display','block')时，会把style里边的样式都以文本的方式显示在桌面上，script中的所有内容也以文本的形式出现在了页面上。  //原因分析:因为html文件中style和script等元素不被解析到页面上的原因是其display属性是none的原因，而使用$(':hidden')的话会获取到所有的display为none的元素，所以会出现上面的那种现象。
  //但是，假如我们只是向上边那样全选隐藏的元素，但是不更改他们的display属性的，style和script中文本不会出来
  // 而使用下边的$(':visible')选择所有元素是没有这些问题的，即便是选择所有可见元素然后给设置隐藏也没问题
  
  //加时间延迟，以及style样式设置为可见，先用jquery设置不可见，再延迟几秒设置为可见，依然会出现这个问题
  ```

  

+ :visible  匹配所有的可见元素



##### 属性选择器补充

+ [attribute]    匹配包含给定属性的元素

  ```js
  $(['div[id]'])   查找所有含有id属性的div元素
  ```

+ [attribute=value]    匹配包含给定属性是某个特定值的元素

  ```
  $("input[name='newsletter']").attr("checked",true)    //查找input标签name='newseletter'并设置为选中(type等属性也是可以的)
  ```

+ [attribute!=value]     匹配不包含给定属性是某个特定值的元素

  ```js
  //不在前边加限制也会导致没有type属性的元素被选中
  ```

+ 其他

  ```
  [attribute^=value]    以某些内容开头的
  [attribute$=value] 	  以某些内容结尾的
  [attribute*=value]    属性值中包含这个内容的
  [attribute^=value][attribute$=value][attribute*=value]    复合，需要同时满足这些条件的
  ```

##### 子元素选择器补充

+ :nth-child(index/even/odd/equation)   匹配其父元素下的第N个子或奇偶元素   (这个equation不知道是啥，写了也会报错其他都没啥问题)
+ :first-child
+ :last-child
+ only-child  如果某个元素是父元素中国唯一的子元素，就匹配，否则，不匹配

###### 表单选择器补充

+ ```
  :input
  :text
  :password
  :redio
  :checkbox
  :submit
  :image
  :reset
  :button
  :file
  :hidden
  ```

+ 表单对象属性选择器

  ```
  :enabled
  :disabled
  :checked
  :selected
  ```

  

