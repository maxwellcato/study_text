### jquery事件

##### jquery事件注册

+ 单个事件注册

  ```shell
  element.事件(function(){}) 
  
  $(“div”).click(function(){  事件处理程序 })
  
  其他事件和原生基本一致。
  比如mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll 等
  1.# 补充: 前台，绑定事件的是A，A里面还有子元素B，那么:
  	# A元素绑定mouseover和mouseout事件时，鼠标由A元素移动到B元素上，这两个事件也会被触发
  	# A元素绑定mouseenter和mouseleave事件时,鼠标从A移动到B上，不会被触发
  2.# 补充:keypress和keydown的区别
  	# 相同点:都是按着键盘不送，一直触发，这跟keyup不一样
  	# 不同点:
  		# 按键盘上的任意键都会触发keydown事件，而keypress只能在按字符间键时触发(如果是拼音输入的话，按字符和空格都不会触发(keydown是全部触发)...)
  		# 因为上一条原因(按功能键不会触发)，keypress和(keydown keyup)的e.keyCode的值并不完全等同(数字键相同，字母键不相同)
  		# 二者的根本区别是, 系统由KeyDown返回键zhi盘的代码, 然后由TranslateMessage函数翻译成成字符, 由KeyPress返回字符值. 因此在KeyDown中返回的是键盘的代码, 而KeyPress返回的是ASCII字符. 所以根据你的目的, 如果只想读取字符, 用KeyPress, 如果想读各键的状态, 用KeyDown.
  	# 按下键盘会触发键盘事件，顺序依次为：keydown->keypress->keyup.
  ```

##### 事件处理

+ 事件处理on()绑定事件

  ```
  on() 方法在匹配元素上绑定一个或多个事件的事件处理函数:
  
  element.on(events,[selector],fn)
  
  1. events:一个或多个用空格分隔的事件类型，如"click"或"keydown" 。
  2. selector: 元素的子元素选择器 。
  3. fn:回调函数 即绑定在元素身上的侦听函数
  ```

  + **2.1** **事件处理** **on()** **绑定事件**

    ```shell
    1.# on() 方法优势1:
    	# 可以绑定多个事件，多个处理事件处理程序
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
    
    ```

  + **2.2** **事件处理** **off()** **解绑事件**

    + off() 方法可以移除通过 on() 方法添加的事件处理程序

      ```shell
          $("p").off() // 解绑p元素所有事件处理程序
      
          $("p").off( "click")  // 解绑p元素上面的点击事件 后面的 foo 是侦听函数名
      
          $("ul").off("click", "li");   // 解绑事件委托
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

      

    