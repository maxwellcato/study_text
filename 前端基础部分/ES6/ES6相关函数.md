### ES6新增

##### 剩余函数

+ ES5中使用的argument对象
	+ 箭头函数没有argument对象
	```javascript
	var fn1 = () => {
		console.log(argument)
	}
	fn1()// => undefined
	```
```
	+ argument除了有length属性外，还拥有callee属性，这个属性指代函数本身，因此可以用于实现递归的调用
	```javascript
	function fn() {
		console.log(arguments.callee)
	};
	fn();//亲测，console.log出来的是这个函数本身
```
+ ES6剩余函数使用方法：
```javascript
var fn1 = function(x1,x2,...rest) {
	console.log(rest)
}
fn1(1,2,3,4,5);// => [3,4,5](rest以数组的形式保存多余参数)
```
	+ 箭头函数可以使用剩余函数
	```
	var fn2 = (...rest) => {
		console.log(rest)
	}
	fn2();// => []
```
##### this对象

+ 函数里边除了有arguments对象，还有this对象





```