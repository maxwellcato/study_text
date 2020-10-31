借鉴抄袭视频链接：https://www.bilibili.com/video/BV15x411f7xe?p=3

### JavaScript知识精炼

#### js的基础数据类型

##### usbnons，使用typeof可进行判断
	+ undefined：函数没有返回值的时候、定义的变量没有赋值
	+ string：字符串类型
	+ boolean：布尔值
	+ number：数字类型
	+ object：对象{}
	+ null：虽然使用typeof(null)返回的是object，但null仍然属于基本数据类型
	+ symbol(ES6新增)
+ js的基本数据类型
	+ undefined
	+ string
	+ boolean
	+ number
	+ null
	+ symbol
+ 基本数据类型存储在栈内存，存储的是值。
+ 复杂数据类型的值存储在堆内存，地址(指向堆中的值)存储在栈内存。当我们把对象赋值给另外一个变量的时候，复制的是地址，指向同一块内存空间，当其中一个对象改变时，另一个对象也会变化。
+PS：function的数据类型是什么？
	+	hr不问就不要说function的数据类型。问的话就说不是
##### NaN	Not a Number
+ isNaN这个函数在ES5中并不靠谱，ES6修复了这个问题
+ NaN 跟谁都不相等，包括它本身
	+ 10/'na' =>NaN
	+ typeof NaN =>'number'
	+ isNaN(16) => false
	+ isNaN(NaN) => true
	+ isNaN('10') => false
+ 服务器返回的数据以字符串居多
	+ NaN == NaN => false
	+ NaN != NaN => true
	+ 因此，在ES5中，可以通过检测一个数据是否等于它本身，如果相等，不是NaN，如果不相等，是NaN
	+ 在ES6中，直接使用isNaN来检测就可以了(isNaN(NaN) =>true
##### js作用域
+ 全局作用域
+ 函数作用域
+ ES6块级作用域
+ 以后能用const的就用const，实在用不了的再用let
##### 作用域链
+ 子函数没有的找父级
	+ 这个找父级跟在哪执行没有关系，而是看它物理的代码位置在哪有关系(因为函数的作用域在定义的时候就确定了)
	
	  ```javascript
	  var a = 666;
	  function show() {
	  var a =12;
	  show2()		//=>666
	  }
	  function show2(){
	  console.log(a)
	  }	
	  //第二种情况
	  var a = 666;
	  function show() {
	  var a =12;
	  function show2(){
	  console.log(a)
	  }
	  show2()		//=>12
	  }
	  ```
	  
##### 函数预解析（hoisting/中文名叫变量预提升）
+ 举例：
	``` javascript
	console.log(numbers);
	var numbers = 12;
	//因为变量预提升的缘故，函数会变成下面这样
	var numbers;
	console.log(numbers);
	numbers = 12;
	```

##### 严格模式
+使用严格模式:
严格模式下，不允许使用未定义的变量，这样可以避免很多未报错的bug
```javascript
"use strict" //在代码刚开始使用，会使用严格模式
```

##### IIFE(匿名函数自执行)
+ 正常函数
```javascript
function show() {
	console.log(123);
};
show(); 	// =>123
```
+ 匿名函数
```javascript
(function(){
	console.log(123);
})();	// => 123
```
+ 使用匿名函数可以防止变量污染。举例:
```javascript
(function(){
	var num1 = 456;
})();
...
(function(){
	var num1 = 123;
})();
```
+ 这种方法在框架和模块化中使用很普遍

##### closure(闭包)
+ 举例：
```javascript
function show(){
	var num = 666;
	return function(){
		console.log(num);
	}
}
var show2 = show();
show2();	// => 666
```
##### DOM事件流
+ 点击一个父元素中的子元素，哪个元素先对点击做出反应？
+ 默认是冒泡，即先子元素再父元素；不过也可以修改成捕获（先父元素再子元素）

##### this相关知识
+ 1.在浏览器下，全局的this 指向 window 举例：
+ 2.在函数的情况下，this 指向:
` show(){console.log(this)};// => Window ` //这个this指向window的本质是直接定义函数是把函数定义到window对象上了，然后调用的话，相当于直接调用`window.show()`,因此指向window
如果是严格模式下，即`use strict`,则 `=> undefined`，本质是严格模式下，直接定义的匿名函数在调用时如果不加window，会返回
+ 3.对象,this 指向 对象
```javascript
'use strict'
var teacer = {
	name: 'leolau',
	showName: function(){
		console.log(this);
	}
};
```
+ 面试题1：
```javascript
'use strict'
var teacer = {
	name: "leolau",
	showName: function(){
		function testThis(){
			console.log(this);
		}
		testThis();//虽然是在对象里调用，但this指的是函数自身被定义时候的上下文中的this，所以这个this指向与函数中的指向相同，均为undefined
	}
}
```
+面试题2(带坑)：
```javascript
var name = 'bilibili'
var teacer = {
	name: "leolau",
	showName: function(){
		function testThis(){
			console.log(this.name);
		}
		testThis();// => 'bilibili'(非严格模式下，function中的this指向window，因为在全局模式中定义了name = 'bilibili',所以输出'bilibili')
	}
}
```
+ 面试题2中this 指向 window的解决办法：
	+ 使用that = this过渡一下
	+ let 接一下
	+ 箭头函数
	+ **<u>使用bind、call、apply</u>**（重点）
		+ bind
			+使用bind(函数需要加一些默认参数时用bind,bind并不立即执行函数，而是将构造函数直接复制成一个新的函数，并改变新的函数的this的指向，使用时需要调用函数)
			
			```javascript
      var name = 'bilibili'
            var teacer = {
                name: "leolau",
                showName: function(){
                    var testThis = function(){
                        console.log(this.name);
                    }.bind(this)
                    testThis();// => 
                }
			      }
			```
		+ call
    	+ call用法：`对象.call(用于矫正的this，参数1，参数2，参数...)`(函数需要直接立刻执行时且加参数用call)
            
        
        ```javascript
        function show(){
                console.log(this，a);
            }
            show.call('哔哩哔哩');
            //所以，对面试题2的修改：
            var name = 'bilibili'
            var teacer = {
            	name: "leolau",
            	showName: function(){
            		function testThis(){
            			console.log(this.name);
		        		}
		        		testThis.call(this);// => 'leolau'
		        	}
		        }
		    ```
		    
		    
		+ apply
			+ 跟call非常相似，只是在添加参数时需要以数组的形式进行添加
			apply(用于矫正的this,[参数1，参数2，参数...])
			```javascript
			var arr = [12,5,8];
			Math.max.apply(arr,arr);//12
			Math.max.apply(null,arr);//12(apply的第一个参数指向谁并不重要)
			//apply的用法体现在这里：
			Math.max(12,8,9)// => 12 (函数只能这样用)
			Math.max([12,8,9])//会报错，添加上apply可使用
			```
##### 面向对象编程和简单的设计模式
+ js里面面向对象编程(oo编程)
+ 创建对象的三种方式
	+ 单体模式:
	```javascript
	var Student = {
		name: 'lihua',
		age: 14,
		showName: function(){
			return this.name;
		}
	};
	Stydent.showName();// => 'lihua'
	```
	+ 原型模式(es5常用)
		+ 属性放在构造函数里，方法放在原型上：
	    ```javascript
		function Teacher(name,age){
			this.name = name;
			this.age = age;
		};
		Teacher.prototype.showName = function(){
			return this.name;
		};
		var xiujing = new Teacher('xiujing',30);
		xiujing.showName();// => 'xiujing'
		  ```
	```
	+ 伪类模式(也叫class(类)模式，es6常用本质还是原型模式，写法不同而已
	​```javascript
	class Teacher{
		constructor(name,age){
			this.name = name;
			this.age = age;
		}
		showName(){
			return this.name
		}
	}
	var chenxi = new Teacher('chenxi',23);
	chenxi.showName();// => 'chenxi'
	```
##### 继承
+ 单体模式下的继承
	+ 代码：
	```javascript
	var Teacher = {
		name: 'mathone',
		age: 26,
		job: '10years',
		showName:function(){
			return this.name;
		}
	};
	var student = Object.create(Teacher);
	student.name = 'yunmeng';
	student.job = '3month';
	console.log(student.showName());// => yunmeng
	student.showJob = function(job){
		return this.job;
	}
	console.log(student.showJob());// => '3month'
	
	```
+ 伪类的继承
	+ 代码：
	```javascript
	class Person{
		constructor(name,age){
			this.name = name;
			this.age = age;
		}
		showName(){
			return this.name;
		}
	};
	class Teacher extends Person{
		constructor(name,age,job){
			super(name,age);
			this.job = job;
		}
		showInfo(){
			return this.job+'---'+super.showName();
		}
	};
	var beibei = new Teacher('beibei',18,'newcoming');
	beibei.showInfo();
	```
##### 数据处理
+ 面试一般问原理，但是你必须会技术(数据交互，跨域)
+ JSONP原理
	+ js是可以跨域的
	+ 服务器返回的数据，show([12,5,8]);
	+ 本地方法的定义 `function show(data){console.log(data)}`
	+ JSONP只能使用get方式
+ CROS
	+ 必须需要服务器端配合，否则没戏
	+ access-allow









