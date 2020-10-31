## jQuery

+ 目前jQuery有1.x和2.x两个主要版本，区别在于2.x移除了对古老的IE 6、7、8的支持，因此2.x的代码更精简。选择哪个版本主要取决于你是否想支持IE 6~8。

+ `$`是著名的jQuery符号。实际上，jQuery把所有功能全部封装在一个全局变量`jQuery`中，而`$`也是一个合法的变量名，它是变量`jQuery`的别名：

  ```javascript
  window.jQuery; // jQuery(selector, context)
  window.$; // jQuery(selector, context)
  $ === jQuery; // true
  typeof($); // 'function'
  ```

+ `$`本质上就是一个函数，但是函数也是对象，于是`$`除了可以直接调用外，也可以有很多其他属性。但是，有时候看到的`$`函数名可能不是`jQuery(selector, context)`，因为很多JavaScript压缩工具可以对函数名和参数改名，所以压缩过的jQuery源码`$`函数可能变成`a(b, c)`。

+ 绝大多数时候，我们都直接用`$`（因为写起来更简单嘛）。但是，如果`$`这个变量不幸地被占用了，而且还不能改，那我们就只能让`jQuery`把`$`变量交出来，然后就只能使用`jQuery`这个变量：

  ```javascript
  $; // jQuery(selector, context)
  jQuery.noConflict();
  $; // undefined
  jQuery; // jQuery(selector, context)
  //这种黑魔法的原理是jQuery在占用$之前，先在内部保存了原来的$,调用jQuery.noConflict()时会把原来保存的变量还原。
  ```

  

### 选择器

+ 基础

  + 如果某个DOM节点有`id`属性，利用jQuery查找如下：

    ```javascript
    // 查找<div id="abc">:
    var div = $('#abc');
    ```

  + 以上面的查找为例，如果`id`为`abc`的`<div>`存在，返回的jQuery对象如下：

    ```html
    [<div id="abc">...</div>]
    <!--如果不存在,则返回：-->
    []
    <!--总之jQuery的选择器不会返回undefined或者null，这样的好处是你不必在下一行判断if (div === undefined)-->
    ```

  + jQuery对象和DOM对象之间可以互相转化：

    ```javascript
    var div = $('#abc'); // jQuery对象
    var divDom = div.get(0); // 假设存在div，获取第1个DOM元素
    var another = $(divDom); // 重新把DOM包装为jQuery对象
    //通常情况下你不需要获取DOM对象，直接使用jQuery对象更加方便。如果你拿到了一个DOM对象，那可以简单地调用$(aDomObject)把它变成jQuery对象，这样就可以方便地使用jQuery的API了。
    ```

  + 按tag查找只需要写上tag名称就可以了：

    ```javascript
    var ps = $('p'); // 返回所有<p>节点
    ps.length; // 数一数页面有多少个<p>节点
    ```

  + 按class查找注意在class名称前加一个`.`

    ```javascript
    var a = $('.red'); // 所有节点包含`class="red"`都将返回
    // 例如:
    // <div class="red">...</div>
    // <p class="green red">...</p>
    
    //通常很多节点有多个class，我们可以查找同时包含`red`和`green`的节点：
    var a = $('.red.green'); // 注意没有空格！
    // 符合条件的节点：
    // <div class="red green">...</div>
    // <div class="blue green red">...</div>
    ```

  + 一个DOM节点除了`id`和`class`外还可以有很多属性，很多时候按属性查找会非常方便，比如在一个表单中按属性来查找

    ```javascript
    var email = $('[name=email]'); // 找出<??? name="email">
    var passwordInput = $('[type=password]'); // 找出<??? type="password">
    var a = $('[items="A B"]'); // 找出<??? items="A B">
    ```

    + 当属性的值包含空格等特殊字符时，需要用双引号括起来。另外，按属性查找还可以使用前缀查找或者后缀查找：

      ```javascript
      var icons = $('[name^=icon]'); // 找出所有name属性值以icon开头的DOM
      // 例如: name="icon-1", name="icon-2"
      var names = $('[name$=with]'); // 找出所有name属性值以with结尾的DOM
      // 例如: name="startswith", name="endswith"
      ```

      这个方法尤其适合通过class属性查找，且不受class包含多个名称的影响：

      ```javascript
      var icons = $('[class^="icon-"]'); // 找出所有class包含至少一个以`icon-`开头的DOM
      // 例如: class="icon-clock", class="abc icon-home"
      ```

  + 组合查找

    + 组合查找就是把上述简单选择器组合起来使用。如果我们查找`$('[name=email]')`，很可能把表单外的`<div name="email">`也找出来，但我们只希望查找`<input>`，就可以这么写：

      ```javascript
      var emailInput = $('input[name=email]'); // 不会找出<div name="email">
      ```

      同样的，根据tag和class来组合查找也很常见：

      ```javascript
      var tr = $('tr.red'); // 找出<tr class="red ...">...</tr>
      ```

  + 多项选择器

    + 多项选择器就是把多个选择器用`,`组合起来一块选：

      ```javascript
      $('p,div'); // 把<p>和<div>都选出来
      $('p.red,p.green'); // 把<p class="red">和<p class="green">都选出来
      ```

      要注意的是，选出来的元素是按照它们在HTML中出现的顺序排列的，而且不会有重复元素。例如，`<p class="red green">`不会被上面的`$('p.red,p.green')`选择两次。

+ 层级选择器

  + 如果两个DOM元素具有层级关系，就可以用`$('ancestor descendant')`来选择，层级之间用空格隔开。例如：

      ```html
      <!-- HTML结构 -->
      <div class="testing">
          <ul class="lang">
              <li class="lang-javascript">JavaScript</li>
              <li class="lang-python">Python</li>
              <li class="lang-lua">Lua</li>
          </ul>
      </div>
      ```

      要选出JavaScript，可以用层级选择器：

      ```javascript
      $('ul.lang li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
      $('div.testing li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
      ```

  + 子选择器（Child Selector）

    + 子选择器`$('parent>child')`类似层级选择器，但是限定了层级关系必须是父子关系，就是``节点必须是``节点的直属子节点。还是以上面的例子：

        ```javascript
        $('ul.lang>li.lang-javascript'); // 可以选出[<li class="lang-javascript">JavaScript</li>]
        $('div.testing>li.lang-javascript'); // [], 无法选出，因为<div>和<li>不构成父子关系
        ```

  + 过滤器（filter）

      + 过滤器一般不单独使用，它通常附加在选择器上，帮助我们更精确地定位元素。观察过滤器的效果：

        ```javascript
        $('ul.lang li'); // 选出JavaScript、Python和Lua 3个节点
        
        $('ul.lang li:first-child'); // 仅选出JavaScript
        $('ul.lang li:last-child'); // 仅选出Lua
        $('ul.lang li:nth-child(2)'); // 选出第N个元素，N从1开始
        $('ul.lang li:nth-child(even)'); // 选出序号为偶数的元素
        $('ul.lang li:nth-child(odd)'); // 选出序号为奇数的元素
        ```

  + 表单相关

    + `:input`：可以选择``，``，``和``；
    + `:file`：可以选择``，和`input[type=file]`一样；
    + `:checkbox`：可以选择复选框，和`input[type=checkbox]`一样；
    + `:radio`：可以选择单选框，和`input[type=radio]`一样；
    + `:focus`：可以选择当前输入焦点的元素，例如把光标放到一个``上，用`$('input:focus')`就可以选出；
    + `:checked`：选择当前勾上的单选框和复选框，用这个选择器可以立刻获得用户选择的项目，如`$('input[type=radio]:checked')`；
    + `:enabled`：可以选择可以正常输入的``、`` 等，也就是没有灰掉的输入；
    + `:disabled`：和`:enabled`正好相反，选择那些不能输入的。

  + 此外，jQuery还有很多有用的选择器，例如，选出可见的或隐藏的元素：

      ```javascript
      $('div:visible'); // 所有可见的div
      $('div:hidden'); // 所有隐藏的div
      ```

+ 查找和过滤

  + 通常情况下选择器可以直接定位到我们想要的元素，但是，当我们拿到一个jQuery对象后，还可以以这个对象为基准，进行查找和过滤。最常见的查找是在某个节点的所有子节点中查找，使用`find()`方法，它本身又接收一个任意的选择器。例如如下的HTML结构：

    ```html
    <!-- HTML结构 -->
    <ul class="lang">
        <li class="js dy">JavaScript</li>
        <li class="dy">Python</li>
        <li id="swift">Swift</li>
        <li class="dy">Scheme</li>
        <li name="haskell">Haskell</li>
    </ul>
    ```

    用`find()`查找：

    ```javascript
    var ul = $('ul.lang'); // 获得<ul>
    var dy = ul.find('.dy'); // 获得JavaScript, Python, Scheme
    var swf = ul.find('#swift'); // 获得Swift
    var hsk = ul.find('[name=haskell]'); // 获得Haskell
    ```

    如果要从当前节点开始向上查找，使用`parent()`方法：

    ```javascript
    var swf = $('#swift'); // 获得Swift
    var parent = swf.parent(); // 获得Swift的上层节点<ul>
    var a = swf.parent('.red'); // 获得Swift的上层节点<ul>，同时传入过滤条件。如果ul不符合条件，返回空jQuery对象
    ```

    对于位于同一层级的节点，可以通过`next()`和`prev()`方法，例如：

    ```javascript
    var swift = $('#swift');
    
    swift.next(); // Scheme
    swift.next('[name=haskell]'); // 空的jQuery对象，因为Swift的下一个元素Scheme不符合条件[name=haskell]
    
    swift.prev(); // Python
    swift.prev('.dy'); // Python，因为Python同时符合过滤器条件.dy
    ```

  + **过滤**，和函数式编程的map、filter类似，jQuery对象也有类似的方法。`filter()`方法可以过滤掉不符合选择器条件的节点：

    ```javascript
    var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
    var a = langs.filter('.dy'); // 拿到JavaScript, Python, Scheme
    ```

    或者传入一个函数，要特别注意函数内部的`this`被绑定为DOM对象，不是jQuery对象：

    ```javascript
    var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
    langs.filter(function () {
        return this.innerHTML.indexOf('S') === 0; // 返回S开头的节点
    }); // 拿到Swift, Scheme
    ```

    `map()`方法把一个jQuery对象包含的若干DOM节点转化为其他对象：

    ```javascript
    var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
    var arr = langs.map(function () {
        return this.innerHTML;
    }).get(); // 用get()拿到包含string的Array：['JavaScript', 'Python', 'Swift', 'Scheme', 'Haskell']
    ```

    此外，一个jQuery对象如果包含了不止一个DOM节点，`first()`、`last()`和`slice()`方法可以返回一个新的jQuery对象，把不需要的DOM节点去掉：

    ```javascript
    var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
    var js = langs.first(); // JavaScript，相当于$('ul.lang li:first-child')
    var haskell = langs.last(); // Haskell, 相当于$('ul.lang li:last-child')
    var sub = langs.slice(2, 4); // Swift, Scheme, 参数和数组的slice()方法一致
    ```

### 操作DOM

+ ### 修改Text和HTML

  + jQuery对象的`text()`和`html()`方法分别获取节点的文本和原始HTML文本

    ```javascript
    $('#test-ul li[name=book]').text(); // 'Java & JavaScript'
    $('#test-ul li[name=book]').html(); // 'Java &amp; JavaScript'
    ```

  + 一个jQuery对象可以包含0个或任意个DOM对象，它的方法实际上会作用在对应的每个DOM节点上。而且jQuery对象的另一个好处是我们可以执行一个操作，作用在对应的一组DOM节点上。即使选择器没有返回任何DOM节点，调用jQuery对象的方法仍然不会报错

+ ### 修改CSS

  + jQuery对象有“批量操作”的特点，这用于修改CSS实在是太方便了

    ```javascript
    $('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');
    ```

    jQuery对象的获取属性时，可以使用`css()`方法这么用：

    ```javascript
    var div = $('#test-div');
    div.css('color'); // '#000033', 获取CSS属性
    div.css('color', '#336699'); // 设置CSS属性
    div.css('color', ''); // 清除CSS属性
    ```

    为了和JavaScript保持一致，CSS属性可以用`'background-color'`和`'backgroundColor'`两种格式。

    `css()`方法将作用于DOM节点的`style`属性，具有最高优先级。如果要修改`class`属性，可以用jQuery提供的下列方法：

    ```javascript
    var div = $('#test-div');
    div.hasClass('highlight'); // false， class是否包含highlight
    div.addClass('highlight'); // 添加highlight这个class
    div.removeClass('highlight'); // 删除highlight这个class
    ```

+ ### 显示和隐藏DOM

  + 要隐藏一个DOM，我们可以设置CSS的`display`属性为`none`，利用`css()`方法就可以实现。不过，要显示这个DOM就需要恢复原有的`display`属性，这就得先记下来原有的`display`属性到底是`block`还是`inline`还是别的值。

  + 考虑到显示和隐藏DOM元素使用非常普遍，jQuery直接提供`show()`和`hide()`方法，我们不用关心它是如何修改`display`属性的，总之它能正常工作：

    ```javascript
    var a = $('a[target=_blank]');
    a.hide(); // 隐藏
    a.show(); // 显示
    //隐藏DOM节点并未改变DOM树的结构，它只影响DOM节点的显示。这和删除DOM节点是不同的。
    ```

+ ### 获取DOM信息

  + 利用jQuery对象的若干方法，我们直接可以获取DOM的高宽等信息，而无需针对不同浏览器编写特定代码:

    ```javascript
    // 浏览器可视窗口大小:
    $(window).width(); // 800
    $(window).height(); // 600
    
    // HTML文档大小:
    $(document).width(); // 800
    $(document).height(); // 3500
    
    // 某个div的大小:
    var div = $('#test-div');
    div.width(); // 600
    div.height(); // 300
    div.width(400); // 设置CSS属性 width: 400px，是否生效要看CSS是否有效
    div.height('200px'); // 设置CSS属性 height: 200px，是否生效要看CSS是否有效
    ```

    `attr()`和`removeAttr()`方法用于操作DOM节点的属性：

    ```javascript
    // <div id="test-div" name="Test" start="1">...</div>
    var div = $('#test-div');
    div.attr('data'); // undefined, 属性不存在
    div.attr('name'); // 'Test'
    div.attr('name', 'Hello'); // div的name属性变为'Hello'
    div.removeAttr('name'); // 删除name属性
    div.attr('name'); // undefined
    ```

    `prop()`方法和`attr()`类似，但是HTML5规定有一种属性在DOM节点中可以没有值，只有出现与不出现两种，例如：

    ```javascript
    <input id="test-radio" type="radio" name="test" checked value="1">
    //  等价于
    <input id="test-radio" type="radio" name="test" checked="checked" value="1">
        
    //  `attr()`和`prop()`对于属性`checked`处理有所不同：
    var radio = $('#test-radio');
    radio.attr('checked'); // 'checked'
    radio.prop('checked'); // true
    
    //prop()返回值更合理一些。不过，用is()方法判断更好：
    
    var radio = $('#test-radio');
    radio.is(':checked'); // true
    //类似的属性还有selected，处理时最好用is(':selected')。
    ```

+ ### 操作表单

  + 对于表单元素，jQuery对象统一提供`val()`方法获取和设置对应的`value`属性：

    ```javascript
    var
        input = $('#test-input'),
        select = $('#test-select'),
        textarea = $('#test-textarea');
    
    input.val(); // 'test'
    input.val('abc@example.com'); // 文本框的内容已变为abc@example.com
    
    select.val(); // 'BJ'
    select.val('SH'); // 选择框已变为Shanghai
    
    textarea.val(); // 'Hello'
    textarea.val('Hi'); // 文本区域已更新为'Hi'
    ```

    

  + 一个`val()`就统一了各种输入框的取值和赋值的问题

### 修改DOM结构

+ ### 添加DOM

  + 要添加新的DOM节点，除了通过jQuery的`html()`这种暴力方法外，还可以用`append()`方法，例如：

    ```javascript
    /*<div id="test-div">
        <ul>
            <li><span>JavaScript</span></li>
            <li><span>Python</span></li>
            <li><span>Swift</span></li>
        </ul>
    </div>*/
    var ul = $('#test-div>ul');
    ul.append('<li><span>Haskell</span></li>');
    ```



### 事件

+ 由于不同的浏览器绑定事件的代码都不太一样，所以用jQuery来写代码，就屏蔽了不同浏览器的差异，我们总是编写相同的代码。

  + 举个例子，假设要在用户点击了超链接时弹出提示框，我们用jQuery这样绑定一个`click`事件

    ```javascript
    /* HTML:
     *
     * <a id="test-link" href="#0">点我试试</a>
     *
     */
    
    // 获取超链接的jQuery对象:
    var a = $('#test-link');
    a.on('click', function () {
        alert('Hello!');
    });
    //on方法用来绑定一个事件，我们需要传入事件名称和对应的处理函数。
    //另一种更简化的写法是直接调用click()方法：
    a.click(function () {
        alert('Hello!');
    });
    //两者完全等价，一般用第二种写法
    ```

+ jQuery能够绑定的事件主要包括：

  + ### 鼠标事件

    + click: 鼠标单击时触发；
    + dblclick：鼠标双击时触发；
    + mouseenter：鼠标进入时触发；
    + mouseleave：鼠标移出时触发；
    + mousemove：鼠标在DOM内部移动时触发；
    + hover：鼠标进入和退出时触发两个函数，相当于mouseenter加上mouseleave。

  + ### 键盘事件（通常是`<input>`和`<textarea>`）

    + keydown：键盘按下时触发；
    + keyup：键盘松开时触发；
    + keypress：按一次键后触发。

  + ### 其他事件

    + focus：当DOM获得焦点时触发；

    + blur：当DOM失去焦点时触发；

    + change：当``、``或``的内容改变时触发；

    + submit：当``提交时触发；

    + ready：当页面被载入并且DOM树完成初始化后触发。

    + 其中，`ready`仅作用于`document`对象。由于`ready`事件在DOM完成初始化后触发，且只触发一次，所以非常适合用来写其他的初始化代码。假设我们想给一个``表单绑定`submit`事件，下面的代码没有预期的效果：

      ```html
      <html>
      <head>
          <script>
              // 代码有误:
              $('#testForm).on('submit', function () {
                  alert('submit!');
              });
          </script>
      </head>
      <body>
          <form id="testForm">
              ...
          </form>
      </body>
      ```

      因为JavaScript在此执行的时候，`<from>`尚未载入浏览器，所以`$('#testForm)`返回`[]`，并没有绑定事件到任何DOM上。所以我们自己的初始化代码必须放到`document`对象的`ready`事件中，保证DOM已完成初始化：

      ```html
      <html>
      <head>
          <script>
              $(document).on('ready', function () {
                  $('#testForm).on('submit', function () {
                      alert('submit!');
                  });
              });
          </script>
      </head>
      <body>
          <form id="testForm">
              ...
          </form>
      </body>
      ```

      由于`ready`事件使用非常普遍，所以可以这样简化：

      ```javascript
      $(document).ready(function () {
          // on('submit', function)也可以简化:
          $('#testForm).submit(function () {
              alert('submit!');
          });
      });
      //进一步简化
      $(function () {
          // init...
      });
      
      //而且还可以反复绑定,它们会依次执行
      $(function () {
          console.log('init A...');
      });
      $(function () {
          console.log('init B...');
      });
      $(function () {
          console.log('init C...');
      });
      ```

+ ### 事件参数

  + 有些事件，如`mousemove`和`keypress`，我们需要获取鼠标位置和按键的值，否则监听这些事件就没什么意义了。所有事件都会传入`Event`对象作为参数，可以从`Event`对象上获取到更多的信息：

    ```javascript
    $(function () {
        $('#testMouseMoveDiv').mousemove(function (e) {
            $('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = ' + e.pageY);
        });
    });
    ```

  + ### 取消绑定

    + 一个已被绑定的事件可以解除绑定，通过`off('click', function)`实现：

      ```javascript
      function hello() {
          alert('hello!');
      }
      
      a.click(hello); // 绑定事件
      
      // 10秒钟后解除绑定:
      setTimeout(function () {
          a.off('click', hello);
      }, 10000);
      ```

      需要特别注意的是，下面这种写法是无效的：

      ```javascript
      // 绑定事件:
      a.click(function () {
          alert('hello!');
      });
      // 解除绑定:
      a.off('click', function () {
          alert('hello!');
      });
      
      /*
       *需要特别注意的是，下面这种写法是无效的：
      */
      
      // 绑定事件:
      a.click(function () {
          alert('hello!');
      });
      // 解除绑定:
      a.off('click', function () {
          alert('hello!');
      });
      
      //这是因为两个匿名函数虽然长得一模一样，但是它们是两个不同的函数对象，off('click', function () {...})无法移除已绑定的第一个匿名函数。
      //为了实现移除效果，可以使用off('click')一次性移除已绑定的click事件的所有处理函数。
      //同理，无参数调用off()一次性移除已绑定的所有类型的事件处理函数。
      ```

  + ### 事件触发条件

    + 触发条件一般都是由用户的点击、输入等操作触发的，但是我们用js代码来操作时，就不会触发，这样虽然提高了用户的安全性，但是对于开发代码人员来说，同样的功能需要再多些一些语句才行

    + 有些时候，我们希望用代码触发`change`事件，可以直接调用无参数的`change()`方法来触发该事件：

      ```javascript
      var input = $('#test-input');
      input.val('change it!');
      input.change(); // 触发change事件
      //input.change()相当于input.trigger('change')，它是trigger()方法的简写。
      ```

    + ### 浏览器安全限制

      + 在浏览器中，有些JavaScript代码只有在用户触发下才能执行，例如，`window.open()`函数：

        ```javascript
        // 无法弹出新窗口，将被浏览器屏蔽:
        $(function () {
            window.open('/');
        });
        ```

      + 这些“敏感代码”只能由用户操作来触发：

        ```javascript
        var button1 = $('#testPopupButton1');
        var button2 = $('#testPopupButton2');
        
        function popupTestWindow() {
            window.open('/');
        }
        
        button1.click(function () {
            popupTestWindow();
        });
        
        button2.click(function () {
            // 不立刻执行popupTestWindow()，3秒后执行:
            setTimeout(popupTestWindow, 3000);
        });
        //当用户点击button1时，click事件被触发，由于popupTestWindow()在click事件处理函数内执行，这是浏览器允许的，而button2的click事件并未立刻执行popupTestWindow()，延迟执行的popupTestWindow()将被浏览器拦截(可能被改了？不管是点击廖老师的演示还是自己写一个文件实验，都是两个都可以弹出新窗口啊)。
        ```

        

### 动画

+ 用JavaScript手动实现动画效果，需要编写非常复杂的代码。如果想要把动画效果用函数封装起来便于复用，那考虑的事情就更多了。使用jQuery实现动画，代码已经简单得不能再简化了：只需要一行代码

+ ### show / hide

  + 直接以无参数形式调用`show()`和`hide()`，会显示和隐藏DOM元素。但是，只要传递一个时间参数进去，就变成了动画：

    ```javascript
    var div = $('#test-show-hide');
    div.hide(3000); // 在3秒钟内逐渐消失
    //时间以毫秒为单位，但也可以是'slow'，'fast'这些字符串：
    ```

    + `toggle()`方法则根据当前状态决定是`show()`还是`hide()`。

+ ### slideUp / slideDown

  + 你可能已经看出来了（我没有，别乱说），`show()`和`hide()`是从左上角逐渐展开或收缩的，而`slideUp()`和`slideDown()`则是在垂直方向逐渐展开或收缩的。

+ ### fadeIn / fadeOut

  + `fadeIn()`和`fadeOut()`的动画效果是淡入淡出，也就是通过不断设置DOM元素的`opacity`属性来实现，而`fadeToggle()`则根据元素是否可见来决定下一步动作

+ ### 自定义动画

  + `animate()`，它可以实现任意动画效果，我们需要传入的参数就是DOM元素最终的CSS状态和时间，jQuery在时间段内不断调整CSS直到达到我们设定的值

    ```javascript
    var div = $('#test-animate');
    div.animate({
        opacity: 0.25,
        width: '256px',
        height: '256px'
    }, 3000); // 在3秒钟内CSS过渡到设定值
    
    //animate()还可以再传入一个函数，当动画结束时，该函数将被调用：
    var div = $('#test-animate');
    div.animate({
        opacity: 0.25,
        width: '256px',
        height: '256px'
    }, 3000, function () {
        console.log('动画已结束');
        // 恢复至初始状态:
        $(this).css('opacity', '1.0').css('width', '128px').css('height', '128px');
    });
    ```

+ ### 串行动画

  + 通过`delay()`方法还可以实现暂停，这样，我们可以实现更复杂的动画效果，而代码却相当简单：

    ```javascript
    var div = $('#test-animates');
    // 动画效果：slideDown - 暂停 - 放大 - 暂停 - 缩小
    div.slideDown(2000)
       .delay(1000)
       .animate({
           width: '256px',
           height: '256px'
       }, 2000)
       .delay(1000)
       .animate({
           width: '128px',
           height: '128px'
       }, 2000);
    }
    //因为动画需要执行一段时间，所以jQuery必须不断返回新的Promise对象才能后续执行操作。简单地把动画封装在函数中是不够的
    ```

+ ### 动画没效果

  + 你可能会遇到，有的动画如`slideUp()`根本没有效果。这是因为jQuery动画的原理是逐渐改变CSS的值，如`height`从`100px`逐渐变为`0`。但是很多不是block性质的DOM元素，对它们设置`height`根本就不起作用，所以动画也就没有效果。

    此外，jQuery也没有实现对`background-color`的动画效果，用`animate()`设置`background-color`也没有效果。这种情况下可以使用CSS3的`transition`实现动画效果。

### AJAX

+ ajax			

  + jQuery在全局对象`jQuery`（也就是`$`）绑定了`ajax()`函数，可以处理AJAX请求。`ajax(url, settings)`函数需要接收一个URL和一个可选的`settings`对象，常用的选项如下：

    + async：是否异步执行AJAX请求，默认为`true`，千万不要指定为`false`；
    + method：发送的Method，缺省为`'GET'`，可指定为`'POST'`、`'PUT'`等；
    + contentType：发送POST请求的格式，默认值为`'application/x-www-form-urlencoded; charset=UTF-8'`，也可以指定为`text/plain`、`application/json`；
    + data：发送的数据，可以是字符串、数组或object。如果是GET请求，data将被转换成query附加到URL上，如果是POST请求，根据contentType把data序列化成合适的格式；
    + headers：发送的额外的HTTP头，必须是一个object；
    + dataType：接收的数据格式，可以指定为`'html'`、`'xml'`、`'json'`、`'text'`等，缺省情况下根据响应的`Content-Type`猜测。

  + 例子

    ```javascript
    var jqxhr = $.ajax('/api/categories', {
        dataType: 'json'
    });
    // 请求已经发送了
    ```

  + 完整例子

    ```javascript
    'use strict';
    function ajaxLog(s) {
        var txt = $('#test-response-text');
        txt.val(txt.val() + '\n' + s);
    }
    $('#test-response-text').val('');
    //-----------分割线------------
    var jqxhr = $.ajax('/api/categories', {
        dataType: 'json'
    }).done(function (data) {
        ajaxLog('成功, 收到的数据: ' + JSON.stringify(data));
    }).fail(function (xhr, status) {
        ajaxLog('失败: ' + xhr.status + ', 原因: ' + status);
    }).always(function () {
        ajaxLog('请求完成: 无论成功或失败都会调用');
    });
    ```

  + ### get

    + 