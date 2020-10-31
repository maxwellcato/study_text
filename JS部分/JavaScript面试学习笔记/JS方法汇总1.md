## 字符串的常用方法

### toUpperCase()、toLowerCase()

```javascript
//该方法并不会修改调用该方法的字符串，而是返回一个新的已经全部大写化了的字符串。
```

### indexOf方法

+ 返回调用该方法的字符串中第一次出现查询字符的位置（数组也可以使用这个函数）

### substring方法(注意string的s是小写)

+ 返回索引区的调用该方法的字符串的子串（前闭后开）

### split方法，分割字符串，返回分割出的数组形式的子字符串

+ 语法：`stringObject.split(separator,howmany)`
+ 参数
  + separator    必需。字符串或正则表达式，从该参数指定的地方分割 stringObject。
  + howmany    可选。该参数可指定返回的数组的最大长度。如果设置了该参数，则返回原未设置该参数时前该参数个字符串组成的数组(即返回前howmany个数组对象)。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。
  + 好比，不加howmany，返回['he','','o wor','d']，如果加了参数3，那么返回['he','','o wor']

## 数组的常用方法

+ 数组可以包含任意数据类型

+ push      添加若干元素到数组的结尾，返回新数组的length属性

+ pop  删除数组中最后的一个元素，并返回删除的元素            arr.pop().pop()的方法是错误的，用这个方法删除多个元素，好像只能通过循环的方法来进行

+ shift   删除数组开头的一个元素，返回被删除的元素

+ unshift  添加若干个元素到数组的开头，并返回新数组的长度

+ 以下方法中未直接注明 `可修改原数组的` 均不会对原数组进行修改

+ indexOf方法

+ slice方法，对应字符串的subString版本，截取array的部分元素，并返回这这部分元素作为一个新数组（该方法并不会修改原始数据，splice方法可以修改原数组）

  + slice()中不写任何参数，返回的是原数组，而splice（）中不写任何参数，返回的是空数组，原数组不变

  + ```javascript
    let arr = [1,2,3]
    let arrCopy = arr.slice()
    arrCopy === arr   //()=> fasle
    
    //产生以上原因的本质是数组只要不是完全的arr2=arr1这样的赋值，在===面前都是false，因为这样数组的内存地址不一样在===面前都为false
    
  let src = ['hello']
    let src1 = src.slice()
    console.log(src === src1)  
    //基本数据类型里和非基本数据类型的区别:
    	//当src = ['hello']时，输出的是false
    	//当src = 'hello'时，输出的是true
    ```
    
    

+ sort方法     不加附加条件时，对数组进行默认排序（），

+ reverse方法    将数组元素进行翻转

+ splice方法 `会修改原数组`  该方法是修改数组的万能方法，

  + splice(参数一，参数二，参数三)

  + splice() 方法向/从数组中添加/删除项目，然后返回被删除的项目，原数组被修改。

    + ```javascript
      arrayObject.splice(index,howmany,item1,.....,itemX)
      index //要删除的数据的位置，使用负数可以从数据结尾处规定位置
      howmany //要删除的项目的数量，如果为0则不删除
      item1,......,itemX  //可选，向数组中添加的数据
      //该函数的返回值为：1.如果有删除的元素，则以数组的形式返回被删除的元素  2.一直都是返回被删除的元素，如果没有的话，则返回空数组
      ```

      

+ concat方法，将两个数组连接到一起    （concat方法并不修改原数组，而是返回一个新数组）（字符串也可以使用concat方法，其会将要接收的元素先变成字符串形式，然后拼接，如果是字符串调用该方法，后面参数跟数组的话，数组间的','也会被添加到字符串中）

  + 实际上，`concat()`方法可以接收任意个元素和`Array`，并且自动把`Array`拆开，然后全部添加到新的`Array`里：

  + ```javascript
    var arr = ['A', 'B', 'C'];
    arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
    //concat并不改变调用自己的数组的值，concat返回的值是拼接上的数组
    //作为数组使用时，concat会保存数据的类型存储到新数组中
    ```

+ `join()`方法是一个非常实用的方法，它把当前`Array`的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：

  + ```javascript
    var arr = ['A', 'B', 'C', 1, 2, 3];
    arr.join('-'); // 'A-B-C-1-2-3'   经过join处理后返回的结果均为string类型，.length为11
    arr.join(''); //ABC123    string类型，.length为6
arr.join(); //A,B,C,1,2,3  也全部都是string，里面的'，'也是string类型，.length为11
    ```

    
    
  + 如果`Array`的元素不是字符串，将自动转换为字符串后再连接

# 看到这里了

## 对象的常用方法

+ JavaScript对象的所有属性都是`字符串`，不过属性对应的值可以是`任意数据类型`

+ 如果我们要检测对象是否拥有某一属性，可以用`in`操作符（从原型上继承的也算）：

+ ```javascript
  '属性' in object   //true表示有这个属性，false表示没有这个属性(in前边的东西一定要以字符串的形式)
  ```

+ 要判断一个属性是否是`xiaoming`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：

+ ```javascript
  var xiaoming = {
      name: '小明'
  };
  xiaoming.hasOwnProperty('name'); // true
  xiaoming.hasOwnProperty('toString'); // false
  ```

+ 使用for(let i in obj)方法进行遍历对象属性，只能用[]，而不能用.来进行的原因解释

+ ```javascript
  let name = {
        i: 'hahaha',
        name: 'xiaoming',
        age: '18',
        fale: 'man'
      }
      for(let i in name){
        console.log(`${i}: ${name.i}`)
        console.log(i+ ': ' +name.i)  //使用.i的话，返回的是obj.i值，因为for(let i in ..)循环中i为字符串形式
        console.log(i+ ': '+ name[i])
        console.log(typeof(i))
        console.log(i)
        console.log('------------')
      }
  
  //输出：
  i: hahaha
  i: hahaha
  i: hahaha
  string
  i
  ------------
  name: hahaha
  name: hahaha
  name: xiaoming
  string
  name
  ------------
  age: hahaha
  age: hahaha
  age: 18
  string
  age
  ------------
  fale: hahaha
  fale: hahaha
  fale: man
  string
  fale
  ------------
  ```

  

## 循环方法

+ `for`循环的3个条件都是可以省略的，如果没有退出循环的判断条件，就必须使用`break`语句退出循环，否则就是死循环：

+ ```javascript
  var x = 0;
  for (;;) { // 将无限循环下去
      if (x > 100) {
          break; // 通过if判断来退出循环
      }
      x ++;
  }
  ```

+ `for`循环的一个变体是`for ... in`循环，它可以把一个对象的所有属性依次循环出来

+ 由于`Array`也是对象，而它的每个元素的索引被视为对象的属性，因此，`for ... in`循环可以直接循环出`Array`的索引

+ *注意*，`for ... in`对`Array`的循环得到的是`String`而不是`Number`

+ while和do...while

## Map和Set

### Map

+ 最新的ES6规范引入了新的数据类型`Map`

+ 初始化`Map`需要一个二维数组，或者直接初始化一个空`Map`。`Map`具有以下方法：

+ ```javascript
  var m = new Map(); // 空Map
  m.set('Adam', 67); // 添加新的key-value
  m.set('Bob', 59);
  m.has('Adam'); // 是否存在key 'Adam': true
  m.get('Adam'); // 67
  m.delete('Adam'); // 删除key 'Adam'
  m.get('Adam'); // undefined
  ```

### Set

+ Set具有以下方法

+ ```javascript
  var s = new Set([1, 2, 3]);
  
  s.add(4);
  s; // Set {1, 2, 3, 4}
  s.add(4);
  s; // 仍然是 Set {1, 2, 3, 4}
  
  s; // Set {1, 2, 3,4}
  s.delete(3);
  s; // Set {1, 2,4}
  ```

+ Set用于数组去重。包含Set转数组，以及数组转Set的基础操作:

+ ```javascript
  var arr=[1,2,1,2,3];
  console.log("初始数组："+arr);
  
  var s=new Set(arr);
  console.log(s);
  
  var arr2=Array.from(s);
  console.log("去重后的数组："+arr2);
  ```

## 新类型：iterable类型

+ 遍历`Array`可以采用下标循环，遍历`Map`和`Set`就无法使用下标。为了统一集合类型，ES6标准引入了新的`iterable`类型，`Array`、`Map`和`Set`都属于`iterable`类型。

+ 具有`iterable`类型的集合可以通过es6新出的方法`for ... of`循环来遍历

+ iterable最好用forEach方法来循环遍历里边的内容，该方法的修改操作实际上是对原数组的备份数组进行操作，并不修改原数组，但如果数组中包含的是对象，因为备份数组和原数组均是指向对象的地址，所以，这个方法是可以修改对象的。该方法并不返回任何数据，forEach()方法：

  + ```javascript
    let a = ['a','b','c']
    a.forEach(function (element, index, array) {
        a[index]+= 'c'
        // element: 指向当前元素的值
        // index: 指向当前索引
        // array: 指向Array对象本身(当采用[index]的方式改变了数组，array随之改变，array是原数组的本体)
        console.log(element + ', index = ' + index+'   array:'+array);
    });
    console.log(a) //['ac','bc','cc']
    //而
    a.forEach(function (element, index, array) {
        element += 'c'
        console.log(element + ', index = ' + index+'   array:'+array);
    });
    console.log(a) //原数组并不改变，且其不会返回参数 ['a','b','c']
    ```

  + 总结：

    + 基本类型我们当次循环拿到的**element**，只是forEach给我们在另一个地方复制创建新元素，是和原数组这个元素没有半毛钱联系的！所以，我们使命给循环拿到的**element**赋值都是无用功！

      专业的概念说就是：JavaScript是有**基本数据类型**与**引用数据类型**之分的。对于基本数据类型：`number`,`string`,`Boolean`,`null`,`undefined`它们在**栈内存中直接存储变量与值**。而Object对象的真正的数据是保存在**堆内存**，**栈内只保存了对象的变量以及对应的堆的地址**，所以操作Object其实就是直接操作了原数组对象本身。

      forEach 的基本原理也是for循环，使用**arr[index]**的形式赋值改变，无论什么就都可以改变了。

+ `Set`与`Array`类似，但`Set`没有索引，因此使用forEach()回调函数时，前两个参数都是元素本身
+ `Map`的回调函数参数依次为`value`、`key`和`map`本身

## 函数

### 函数的定义和调用

+ 函数定义方式，两种：

  + ```javascript
    function abs(x) {
        if (x >= 0) {
            return x;
        } else {
            return -x;
        }
    }
    ```

    + JavaScript的函数也是一个对象，上面定义的`abs()`函数实际上是一个函数对象，而函数名`abs`可以视为指向该函数的变量，所以，这两种定义方法是完全等价的。

  + ```javascript
    var abs = function (x) {
        if (x >= 0) {
            return x;
        } else {
            return -x;
        }
    };
    ```

+ JavaScript还有一个免费赠送的关键字`arguments`，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。`arguments`类似`Array`但它不是一个`Array`，它只有length属性，且可以通过下角标来访问

+ ES6标准引入了rest参数，其用法为：

  + ```javascript
    function foo(a, b, ...rest) {
        console.log('a = ' + a);  
        console.log('b = ' + b);
        console.log(rest);
    }
    
    foo(1, 2, 3, 4, 5);
    // 结果:
    // a = 1
    // b = 2
    // [ 3, 4, 5 ]
    foo(1);
    //结果:
    // a = 1
    // b = undefined
    // []
    ```

### 变量作用域与解构赋值

  + JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有使用var申明的变量“提升”到函数顶部：

  	+ var定义的数据只提升定义，不提升赋值。所以在赋值语句前使用变量，并不报错，变量的值为undefined
  
  	+ let和const并没有这个问题，定义前使用会报错
  
  + 解构赋值
  
    + ```javascript
      let [x,y,z] = ['h','a','e']
      console.log(x,y,z) //h a e
      ```
    
    + 解构赋值还可以忽略某些元素：
    
      + ```javascript
        let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
        z; // 'ES6'
        ```
    
    + 对象也可以用解构赋值，如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性：
    
      + ```javascript
        
        var person = {
            name: '小明',
            age: 20,
            gender: 'male',
            passport: 'G-12345678',
            school: 'No.4 middle school'
        };
        var {name, age, passport} = person;
        ```
    
    + 对一个对象进行解构赋值时，同样可以直接对嵌套的对象属性进行赋值，只要保证对应的层次是一致的：
    
      + ```javascript
        var person = {
            name: '小明',
            age: 20,
            gender: 'male',
            passport: 'G-12345678',
            school: 'No.4 middle school',
            address: {
                city: 'Beijing',
                street: 'No.1 Road',
                zipcode: '100001'
            }
        };
        var {name, address: {city, zip}} = person;
        name; // '小明'
        city; // 'Beijing'
        zip; // undefined, 因为属性名是zipcode而不是zip
        // 注意: address不是变量，而是为了让city和zip获得嵌套的address对象的属性:
        address; // Uncaught ReferenceError: address is not defined
        ```
    
    + 使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为`undefined`，这和引用一个不存在的属性获得`undefined`是一致的。如果要使用的变量名和属性名不一致，可以用下面的语法获取：
    
      + ```javascript
        var person = {
            name: '小明',
            age: 20,
            gender: 'male',
            passport: 'G-12345678',
            school: 'No.4 middle school'
        };
        
        // 把passport属性赋值给变量id:
        let {name, passport:id} = person;
        name; // '小明'
        id; // 'G-12345678'
        // 注意: passport不是变量，而是为了让变量id获得passport属性:
        passport; // Uncaught ReferenceError: passport is not defined
        ```
    
    + 解构赋值还可以使用默认值，这样就避免了不存在的属性返回`undefined`的问题
    
      + ```javascript
        var person = {
            name: '小明',
            age: 20,
            gender: 'male',
            passport: 'G-12345678'
        };
        
        // 如果person对象没有single属性，默认赋值为true:
        var {name, single=true} = person;  //如果这里给name也赋一个默认值的话，name被赋的值会是person中name的值，
        name; // '小明' ，即便name给个其他的默认值，这里还是会赋予person中name的值，即默认赋值在对象中不存在该属性时才生效
        single; // true
        ```
    
    + 有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：
    
      + 解决办法是将整个赋值语句用小括号括起来，产生的原因是引擎把‘{’当做了块来处理，而按照块的逻辑，赋值的上下文环境中，‘=’是不对的
    
    + 常见的使用场景
    
      + 两个变量互换值
    
        + ```javascript
          var x=1, y=2;
          [x, y] = [y, x]
          ```
    
      + 快速获取当前页面的域名和路径：
    
        + ```javascript
          var {hostname:domain, pathname:path} = location;
          ```
    
      + 如果一个函数接收一个对象作为参数，那么，可以使用解构直接把对象的属性绑定到变量中。例如，下面的函数可以快速创建一个`Date`对象
    
        + ```javascript
          function buildDate({year, month, day, hour=0, minute=0, second=0}) {
              return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
          }
          ```
    
        + 它的方便之处在于传入的对象只需要`year`、`month`和`day`这三个属性（我记得es6现在默认支持这种用法了，所以解构赋值的这个优点已经不存在了，当然其他优点还是很香的）：
    
          + ```javascript
            buildDate({ year: 2017, month: 1, day: 1 });
            // Mon Sep 28 2020 00:00:00 GMT+0800 (中国标准时间)
            ```
    
          + ```javascript
            buildDate({ year: 2017, month: 1, day: 1, hour: 20, minute: 15 });
            // Mon Sep 28 2020 23:30:26 GMT+0800 (中国标准时间)
            ```
    
  + 方法
  
    + apply
    
      + apply:方法能劫持另外一个对象的方法，继承另外一个对象的属性.
    
      + apply在调用时可以将一个数组默认的转换为一个参数列表
    
      + 因为Math.max 参数里面不支持Math.max([param1,param2]) 也就是数组
    
        但是它支持Math.max(param1,param2,param3…),所以可以根据刚才apply的那个特点来解决 var max=Math.max.apply(null,array),这样轻易的可以得到一个数组中最大的一项(apply会将一个数组装换为一个参数接一个参数的传递给方法)
    
      +  Array.prototype.push 可以实现两个数组合并
    
        同样push方法没有提供push一个数组,但是它提供了push(param1,param,…paramN) 所以同样也可以通过apply来装换一下这个数组,即:
    
        + ```javascript
          var arr1=new Array("1","2","3");
          var arr2=new Array("4","5","6");
          Array.prototype.push.apply(arr1,arr2);
          ```
    
      + apply的装饰器使用
    
        + 对parseInt()函数进行修改，使得其可以记录其被调用了多少次
    
          + ```javascript
                let count = 0
                let oldParseInt = parseInt  //重定义parseInt的时候需要返回parseInt的原功能，如果定义的时候调用parseInt，会导致无限调用自己，且无法停止，导致内存崩掉报错
                window.parseInt = function(){
                  count++
                  return oldParseInt.apply(null,arguments) //调用原函数
                }
                parseInt('10');
                parseInt('20');
                parseInt('30');
                console.log('count = ' + count); // 3
            ```
    
            
    
    + call
    
      + 和apply方法功能相同，只是用法有些不同
    
    + 两种方法详解的链接： https://www.cnblogs.com/chenhuichao/p/8493095.html
    
    + this这个 keyword非常的困惑,但是其实有一个好方法可以理解.
    
      + 检查 ' . ' 左边是谁引用了这个函数. 例如 xiaoming.age(); age函数里面有this, 然后 '. ' 旁边是xiaoming , 那么this就是指向xiaoming了.这种叫做 Implicit Binding.
      + 如果点旁边没有,那就检查有没有用到 bind, apply, call 这三种, 有的话就是调用此方法的对象. 这种叫做 explicit binding.
      + 如果上面两个都没有就检查代码里面有没有用到new 这个keyword, 有的话那就是指向new旁边的函数对象. 这种叫做new binding
      + 上面三个都没有, 检查是不是有arrow function, 有arrow function的话就是, 那么指向是arrow function的lexical binding 的对象. 就是她的parent. 这种叫做 lexical binding
      + 全部都没有如果不是strict mode那就是window对象了.. strict就是 error (undefined).
    
### 高阶函数

+ map和reduce
  + map：把f(x)作用在Array的每一个元素并把结果生成一个新的Array
  
    + map方法的函数只要一个参数，但是map传递的参数有三个，分别是：数组元素、元素索引，数组本身，而paresInt是可以接受两个参数的，第一个是字符，第二个是进制。因此：
  
    + ```javascript
      ['1', '2', '3'].map(paresInt)  
      //相当于
      ['1', '2', '3'].map(paresInt(value, index))
      //执行的内在逻辑就是：
      //第一个元素就是：paresInt('1', 0)  > 1
      //第二个元素就是：paresInt('2', 1)  >  NaN  意思是以一进制审视‘2’，将其转化为10进制输出
      //第三个元素就是：paresInt('3', 2)  >  NaN  意思是以二进制审视‘3’，将其转化为10进制输出
      ```
  
    + .map(parseInt())问题详解链接：https://www.cnblogs.com/guorange/p/7151265.html
  
  + reduce：Array的`reduce()`把一个函数作用在这个`Array`的`[x1, x2, x3...]`上，这个函数必须接收两个参数，`reduce()`把结果继续和序列的下一个元素做累积计算
  
+ filter

  + 把`Array`的某些元素过滤掉，然后返回剩下的元素
  
  + 高级用法
  
    + ```javascript
      var arr = ['A', 'B', 'C'];
      var r = arr.filter(function (element, index, self) {
          console.log(element); // 依次打印'A', 'B', 'C'
          console.log(index); // 依次打印0, 1, 2
          console.log(self); // self就是变量arr，即调用filter的对象
          return true;
      });
      ```
  
  + 去除重复的元素
  
    + ```javascript
      arr.filter(function (element, index, self) {
          return self.indexOf(element) === index;
      })
      ```
  
+ sort
  
  + 排序算法	
  + 默认情况下槽点极多（优先将元素转换成字符串，而后按照ASCII值由小到大排），建议括号里面加上函数，自定义排序规则
  
+ Array
  
  + every    `every()`方法可以判断数组的所有元素是否满足测试条件。满足返回true，不满足返回false
  + find    `find()`方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回`undefined`
  + findIndex    `findIndex()`和`find()`类似，也是查找符合条件的第一个元素，不同之处在于`findIndex()`会返回这个元素的索引，如果没有找到，返回`-1`：
  + forEach    `forEach()`和`map()`类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。`forEach()`常用于遍历数组，因此，传入的函数不需要返回值
  
+ 闭包
  
  + 链接（信息量比较大，就不整个摘录过来了，没事点开链接多看看参悟参悟）：https://www.liaoxuefeng.com/wiki/1022910821149312/1023021250770016
  
  + 函数作为返回值
  
  + 闭包还可以把多参数的函数变成单参数的函数。例如，要计算xy可以用`Math.pow(x, y)`函数，不过考虑到经常计算x2或x3，我们可以利用闭包创建新的函数`pow2`和`pow3`：
  
    + ```javascript
      'use strict';
      
      function make_pow(n) {
          return function (x) {
              return Math.pow(x, n);
          }
      }
      ```
  
    + ```javascript
      // 创建两个新函数:
      var pow2 = make_pow(2);
      var pow3 = make_pow(3);
      
      console.log(pow2(5)); // 25
      console.log(pow3(7)); // 343
      ```
  
+ 箭头函数
  
  + 由于`this`在箭头函数中已经按照词法作用域绑定了，所以，用`call()`或者`apply()`调用箭头函数时，无法对`this`进行绑定，即传入的第一个参数被忽略：
  
+ generator
  
  + generator（生成器）是ES6标准引入的新的数据类型。一个generator看上去像一个函数，但可以返回多次。
  + generator和函数不同的是，generator由`function*`定义（注意多出的`*`号），并且，除了`return`语句，还可以用`yield`返回多次
  + 详情讲解链接：https://www.liaoxuefeng.com/wiki/1022910821149312/1023024381818112
  
### 标准对象

+ 基础知识

  + 包装对象

    + 创建方式

      ```javascript
      var n = new Number(123); // 123,生成了新的包装类型
      var b = new Boolean(true); // true,生成了新的包装类型
      var s = new String('str'); // 'str',生成了新的包装类型
      ```

    + 虽然包装对象看上去和原来的值一模一样，显示出来也是一模一样，但他们的类型已经变为`object`了！所以，包装对象和原始值用`===`比较会返回`false`

    + 所以*闲的蛋疼也不要使用包装对象*！尤其是针对`string`类型！！！

      如果把`new`去掉，`Number()`、`Boolean`和`String()`被当做普通函数，把任何类型的数据转换为`number`、`boolean`和`string`类型（注意不是其包装类型）

  + 建议

    + 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
    + 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
    + 用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
    + 通常不必把任意类型转换为`boolean`再判断，因为可以直接写`if (myVar) {...}`；
    + `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`；
    + 判断`Array`要使用`Array.isArray(arr)`；
    + 判断`null`请使用`myVar === null`；
    + 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
    + 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。
    + 最后有细心的同学指出，任何对象都有`toString()`方法吗？`null`和`undefined`就没有！确实如此，这两个特殊值要除外，虽然`null`还伪装成了`object`类型。
    + 更细心的同学指出，`number`对象调用`toString()`报SyntaxError

  + 转换规则

    + 1  Number Boolean String Undefined 会先转换为数字再比较

      2 Undefined 是基本类型，它转换成数字是 NaN，NaN 和谁对比结果都是 false，包括自己

      3  基本类型和复合类型比较时，会先将复合对象转成基本对象，即依次调用 ValueOf() 和 toString()，最后转为数字

      4  Null 是复合对象，它没有 ValueOf() 和 toString()，它只有与 undefind 和自己对比时结果是 true

+ Date

  + 看链接吧，内容比较多（https://www.liaoxuefeng.com/wiki/1022910821149312/1023021626575072）

  + 常用方法：

    + ```javascript
      var now = new Date();
      now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
      now.getFullYear(); // 2015, 年份
      now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
      now.getDate(); // 24, 表示24号
      now.getDay(); // 3, 表示星期三
      now.getHours(); // 19, 24小时制
      now.getMinutes(); // 49, 分钟
      now.getSeconds(); // 22, 秒
      now.getMilliseconds(); // 875, 毫秒数
      now.getTime(); // 1435146562875, 以number形式表示的时间戳
      ```

+ RegExp

  + 在正则表达式中，如果直接给出字符，就是精确匹配。用`\d`可以匹配一个数字，`\w`可以匹配一个字母或数字

  + `.`可以匹配任意字符

  + 要匹配变长的字符，在正则表达式中，用`*`表示任意个字符（包括0个），用`+`表示至少一个字符，用`?`表示0个或1个字符，用`{n}`表示n个字符，用`{n,m}`表示n-m个字符

  + JavaScript有两种方式创建一个正则表达式（两种方法效果完全一样）：

    + 第一种方式是直接通过`/正则表达式/`写出来
    + 第二种方式是通过`new RegExp('正则表达式')`创建一个RegExp对象

  + RegExp对象的`test()`方法用于测试给定的字符串是否符合条件。

    + ```javascript
      var re = /^\d{3}\-\d{3,8}$/;
      re.test('010-12345'); // true
      re.test('010-1234x'); // false
      re.test('010 12345'); // false
      ```

    + 切分字符串
  
      + 用正则表达式切分字符串比用固定的字符更灵活，如split，字符串无法识别出连续的空格，而正则可以区分
    
    + 分组
    
      + 除了简单地判断是否匹配之外，正则表达式还有提取子串的强大功能。用`()`表示的就是要提取的分组（Group）比如：
    
        `^(\d{3})-(\d{3,8})$`分别定义了两个组，可以直接从匹配的字符串中提取出区号和本地号码：
    
    + 贪婪匹配
    
      + 需要特别指出的是，正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符。举例如下，匹配出数字后面的`0`：
    
        ```javascript
        var re = /^(\d+)(0*)$/;
        re.exec('102300'); // ['102300', '102300', '']
        ```
    
    + 全局匹配
    
      + JavaScript的正则表达式还有几个特殊的标志，最常用的是`g`，表示全局匹配
    
        + ```javascript
          var r1 = /test/g;
          // 等价于:
          var r2 = new RegExp('test', 'g');
          ```
    
      + 正则表达式还可以指定`i`标志，表示忽略大小写，`m`标志，表示执行多行匹配。
  
+ JSON

  + JSON是JavaScript Object Notation的缩写，它是一种数据交换格式。
  + 在JSON中，一共就这么几种数据类型：
    + number：和JavaScript的`number`完全一致；
    + boolean：就是JavaScript的`true`或`false`；
    + string：就是JavaScript的`string`；
    + null：就是JavaScript的`null`；
    + array：就是JavaScript的`Array`表示方式——`[]`；
    + object：就是JavaScript的`{ ... }`表示方式。
    + 以及上面的任意组合
  + JSON还定死了字符集必须是UTF-8，表示多语言就没有问题了。为了统一解析，JSON的字符串规定必须用双引号`""`，Object的键也必须用双引号`""`。
  + JSON.stringify()接收三个参数
    + 第一个是要转化的对象
    + 第二个参数用于控制如何筛选对象的键值，如果我们只想输出指定的属性，可以传入`Array`,数组中填写需要展示的属性（['']，这里边写属性key的话必须要加引号）
    + 第三个是缩进，一般用四个空格组成的字符串：‘    ’
    + JSON.stringify的第二个参数还可以是一个函数，传入两个参数，key和value，这样可对对象的属性和值做一定的操作。
      + 如果我们还想要精确控制如何序列化小明，可以给`xiaoming`定义一个`toJSON()`的方法，直接返回JSON应该序列化的数据
  + JSON.parse()，反序列化，将被JSON.stringify转化后的结果{"key":"value"...}格式的数据转换为对象，这两个是互逆的操作。
  + JSON.parse()，也可以接受一个函数，用来转换解析出的属性
  
### 面向对象编程

+ 基础知识
  + 面向对象编程语言（c#、java）的面向对象编程基本概念
  	+ 类：类是对象的类型模板，例如，定义`Student`类来表示学生，类本身是一种类型，`Student`表示学生类型，但不表示任何具体的某个学生；
  	+ 实例：实例是根据类创建的对象，例如，根据`Student`类可以创建出`xiaoming`、`xiaohong`、`xiaojun`等多个实例，每个实例表示一个具体的学生，他们全都属于`Student`类型。
  	
  + 但是在js中，并不是通过这两个概念来面向对象编程的。JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程
  
    + ```javascript
      var Student = {
          name: 'Robot',
          height: 1.2,
          run: function () {
              console.log(this.name + ' is running...');
          }
      };
      
      var xiaoming = {
          name: '小明'
      };
      
      xiaoming.__proto__ = Student;
      //注意最后一行代码把xiaoming的原型指向了对象Student，看上去xiaoming仿佛是从Student继承下来的：
      ```
  
    + 注意，上边的`__proto__`方法主要是为了讲解，该方法存在严重的兼容问题，绑定原型一般不这么用。而是用Object.create()的方法：
  
      ```javascript
      // 原型对象:
      var Student = {
          name: 'Robot',
          height: 1.2,
          run: function () {
              console.log(this.name + ' is running...');
          }
      };
      
      function createStudent(name) {
          // 基于Student原型创建一个新对象:
          var s = Object.create(Student);
          // 初始化新对象:
          s.name = name;
          return s;
      }
      
      var xiaoming = createStudent('小明');
      xiaoming.run(); // 小明 is running...
      xiaoming.__proto__ === Student; // true
      ```
  
      
  
+ 创建对象

  + 构造函数

    + 除了直接用`{ ... }`创建一个对象外，JavaScript还可以用一种构造函数的方法来创建对象。它的用法是，先定义一个构造函数：

      ```javascript
      function Student(name) {
          this.name = name;
          this.hello = function () {
              alert('Hello, ' + this.name + '!');
          }
      }
      //这是一个普通函数，但是在JavaScript中，可以用关键字new来调用这个函数，并返回一个对象：
      ```

      ```javascript
      var xiaoming = new Student('小明');
      xiaoming.name; // '小明'
      xiaoming.hello(); // Hello, 小明!
      //注意，如果不写new，这就是一个普通函数，它返回undefined。但是，如果写了new，它就变成了一个构造函数，它绑定的this指向新创建的对象，并默认返回this，也就是说，不需要在最后写return this;。
      ```

    + 新创建的`xiaoming`的原型链是：

      ```javascript
      xiaoming ----> Student.prototype ----> Object.prototype ----> null
      //xiaoming的原型指向函数Student的原型。如果你又创建了xiaohong、xiaojun，那么这些对象的原型与xiaoming是一样的：
      xiaoming ↘
      xiaohong -→ Student.prototype ----> Object.prototype ----> null
      xiaojun  ↗
      //用new Student()创建的对象还从原型上获得了一个constructor属性，它指向函数Student本身：
      xiaoming.constructor === Student.prototype.constructor; // true
      Student.prototype.constructor === Student; // true
      Object.getPrototypeOf(xiaoming) === Student.prototype; // true
      xiaoming instanceof Student; // true
      // instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。
      ```

    + 但是

      ```javascript
      xiaoming.name; // '小明'
      xiaohong.name; // '小红'
      xiaoming.hello; // function: Student.hello()
      xiaohong.hello; // function: Student.hello()
      xiaoming.hello === xiaohong.hello; // false
      ```

      + `xiaoming`和`xiaohong`各自的`name`不同，这是对的，否则我们无法区分谁是谁了。

        `xiaoming`和`xiaohong`各自的`hello`是一个函数，但它们是两个不同的函数，虽然函数名称和代码都是相同的！

        如果我们通过`new Student()`创建了很多对象，这些对象的`hello`函数实际上只需要共享同一个函数就可以了，**这样可以节省很多内存。**

      + 要让创建的对象共享一个`hello`函数，根据对象的属性查找原则，我们只要把`hello`函数移动到`xiaoming`、`xiaohong`这些对象共同的原型上就可以了，也就是`Student.prototype`：

        ```javascript
        function Student(name) {
            this.name = name;
        }
        
        Student.prototype.hello = function () {
            alert('Hello, ' + this.name + '!');
        };
        ```

    + 我们还可以编写一个`createStudent()`函数，在内部封装所有的`new`操作。一个常用的编程模式像这样：

      ```javascript
      function Student(props) {
          this.name = props.name || '匿名'; // 默认值为'匿名'
          this.grade = props.grade || 1; // 默认值为1
      }
      
      Student.prototype.hello = function () {
          alert('Hello, ' + this.name + '!');
      };
      
      function createStudent(props) {
          return new Student(props || {})
      }
      ```

      + 这个`createStudent()`函数有几个巨大的优点：一是不需要`new`来调用，二是参数非常灵活，可以不传，也可以这么传：

        ```
        var xiaoming = createStudent({
            name: '小明'
        });
        
        xiaoming.grade; // 1
        ```

+ 原型继承

  + 首先创建一个Student构造函数

    ```javascript
    function Student(props) {
        this.name = props.name || 'Unnamed';
    }
    
    Student.prototype.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
    ```

  + 然后基于`Student`扩展出`PrimaryStudent`，可以先定义出`PrimaryStudent`：

    ```javascript
    function PrimaryStudent(props) {
        // 调用Student构造函数，绑定this变量:
        Student.call(this, props);  //这个this指调用这个函数对象
        this.grade = props.grade || 1;
    }
    ```

    + 但此时，PrimaryStudent的原型链为：

      ```javascript
      new PrimaryStudent() ----> PrimaryStudent.prototype ----> Object.prototype ----> null
      //而我们需要的是：
      new PrimaryStudent() ----> PrimaryStudent.prototype ----> Student.prototype ----> Object.prototype ----> null
      ```

    + 操作：很复杂，而且前边的也看的似是而非，附上链接，整个讲解都去多看看：https://www.liaoxuefeng.com/wiki/1022910821149312/1023021997355072

+ class继承

  + 上边的继承写法不仅难以理解，而且还需要大量的代码来实现，所以，新的关键字`class`从ES6开始正式被引入到JavaScript中。`class`的目的就是让定义类更简单。

    + 用函数实现`Student`的方法：

      ```javascript
      function Student(name) {
          this.name = name;
      }
      
      Student.prototype.hello = function () {
          alert('Hello, ' + this.name + '!');
      }
      ```

    + 用新的`class`关键字来编写`Student`:

      ```javascript
      class Student {
          constructor(name) {	
              this.name = name;
          }
      	//没有逗号	
          hello() {
              alert('Hello, ' + this.name + '!');
          }
      }
      //最后，创建一个xiaoming对象的使用方法和前边的一样
      var xiaoming = new Student('小明');
      xiaoming.hello();
      ```

  + class继承

    + 通过`extends`来实现

      ```javascript
      class PrimaryStudent extends Student {
          constructor(name, grade) {
              super(name); // 记得用super调用父类的构造方法!
              this.grade = grade;  //自己
          }
      
          myGrade() {
              alert('I am at grade ' + this.grade);
          }
      }
      ```

      


### 浏览器

+ 浏览器对象

  + window

    + `window`对象不但充当全局作用域，而且表示浏览器窗口。
    + `window`对象有`innerWidth`和`innerHeight`属性，可以获取浏览器窗口的内部宽度和高度。内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。
    + 对应的，还有一个`outerWidth`和`outerHeight`属性，可以获取浏览器窗口的整个宽高。

  + navigator

    + `navigator`对象表示浏览器的信息，最常用的属性包括：

      ```
      navigator.appName：浏览器名称；
      navigator.appVersion：浏览器版本；
      navigator.language：浏览器设置的语言；
      navigator.platform：操作系统类型；
      navigator.userAgent：浏览器设定的User-Agent字符串。
      ```

    + *请注意*，`navigator`的信息可以很容易地被用户修改，所以JavaScript读取的值不一定是正确的。很多初学者为了针对不同浏览器编写不同的代码，喜欢用`if`判断浏览器版本。但这样既可能判断不准确，也很难维护代码。正确的方法是充分利用JavaScript对不存在属性返回`undefined`的特性，直接用短路运算符`||`计算。

      ```javascript
      var width = window.innerWidth || document.body.clientWidth;
      ```

  + screen

    + `screen`对象表示电脑屏幕的信息（即电脑分辨率1366 x 768），常用的属性有：

      ```javascript
      //screen.width：屏幕宽度，以像素为单位；
      //screen.availWidth:屏幕可用宽度，指去掉window系统页面底部的任务栏，如果任务栏设置为隐藏，两者相同
      //screen.height：屏幕高度，以像素为单位；
      //screen.colorDepth：返回颜色位数，如8、16、24。
      ```

  + location

    + `location`对象表示当前页面的URL信息。例如，一个完整的URL：

      ```javascript
      http://www.example.com:8080/path/index.html?a=1&b=2#TOP
      //可以用location.href来获取,而要获得URL各个部分的值，可以这么写
      location.protocol; // 'http'
      location.host; // 'www.example.com'
      location.port; // '8080'
      location.pathname; // '/path/index.html'
      location.search; // '?a=1&b=2'
      location.hash; // 'TOP'
      ```

    + 要加载一个新页面，可以调用`location.assign()`。如果要重新加载当前页面，调用`location.reload()`方法非常方便。

  + document

    + `document`对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，`document`对象就是整个DOM树的根节点

    + `document`的`title`属性是从HTML文档中的`xxx`读取的，但是可以动态改变

      ```javascript
      document.title = "这周找到6k以上的工作";
      ```

      

    + `document`对象还有一个`cookie`属性，可以获取当前页面的Cookie。

      ```javascript
      document.cookie; // 'v=123; remember=true; prefer=zh'
      ```

      + 但由于JavaScript能读取到页面的Cookie，而用户的登录信息通常也存在Cookie中，这就造成了巨大的安全隐患，这是因为在HTML页面中引入第三方的JavaScript代码是允许的

      + 为了解决这个问题，服务器在设置Cookie时可以使用`httpOnly`，设定了`httpOnly`的Cookie将不能被JavaScript读取。这个行为由浏览器实现，主流浏览器均支持`httpOnly`选项，IE从IE6 SP1开始支持。

        为了确保安全，服务器端在设置Cookie时，应该始终坚持使用`httpOnly`。

    + history

      + 任何情况，你都不应该使用`history`这个对象了.

+ 操作DOM

  + 基础

    + 始终记住DOM是一个树形结构。操作一个DOM节点实际上就是这么几个操作：

      ```javascript
      //更新：更新该DOM节点的内容，相当于更新了该DOM节点表示的HTML的内容；
      //遍历：遍历该DOM节点下的子节点，以便进行进一步操作；
      //添加：在该DOM节点下新增一个子节点，相当于动态增加了一个HTML节点；
      //删除：将该节点从HTML中删除，相当于删掉了该DOM节点的内容以及它包含的所有子节点。
      ```

    + 在操作一个DOM节点前，我们需要通过各种方式先拿到这个DOM节点

      + 严格地讲，我们这里的DOM节点是指`Element`，但是DOM节点实际上是`Node`，在HTML中，`Node`包括`Element`、`Comment`、`CDATA_SECTION`等很多种，以及根节点`Document`类型，但是，绝大多数时候我们只关心`Element`，也就是实际控制页面结构的`Node`，其他类型的`Node`忽略即可。根节点`Document`已经自动绑定为全局变量`document`。

  + 更新DOM

    + 直接修改节点的文本有两种方法
      1. 修改`innerHTML`属性，这个方式非常强大，不但可以修改一个DOM节点的文本内容，还可以直接通过HTML片段修改DOM节点内部的子树
         1. 用`innerHTML`时要注意，是否需要写入HTML。如果写入的字符串是通过网络拿到了，要注意对字符编码来避免XSS攻击
      2. 第二种是修改`innerText`或`textContent`属性，这样可以自动对字符串进行HTML编码，保证无法设置任何HTML标签
         1. innerText和textContent的区别在于读取属性时，`innerText`不返回隐藏元素的文本，而`textContent`返回所有文本。另外注意IE<9不支持`textContent`。
      
    + 修改CSS也是经常需要的操作。DOM节点的`style`属性对应所有的CSS，可以直接获取或设置。因为CSS允许`font-size`这样的名称，但它并非JavaScript有效的属性名，所以需要在JavaScript中改写为驼峰式命名`fontSize`
    
  + 插入DOM

    + 有两个方法可以插入节点

      1. 使用`appendChild`，把一个子节点添加到父节点的最后一个子节点。更多的时候我们会从零创建一个新的节点，然后插入到指定位置

         1. 动态创建一个节点然后添加到DOM树中，可以实现很多功能。举个例子，下面的代码动态创建了一个``节点，然后把它添加到``节点的末尾，这样就动态地给文档添加了新的CSS定义

            ```javascript
            var d = document.createElement('style');
            d.setAttribute('type', 'text/css');
            d.innerHTML = 'p { color: red }';
            document.getElementsByTagName('head')[0].appendChild(d);
            ```

      2. 使用insertBefore

         1. 使用`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到`referenceElement`之前。

  + 删除DOM

    + 要删除一个节点，首先要获得该节点本身以及它的父节点，然后，调用父节点的`removeChild`把自己删掉

      ```javascript
      // 拿到待删除节点:
      var self = document.getElementById('to-be-removed');
      // 拿到父节点:
      var parent = self.parentElement;
      // 删除:
      var removed = parent.removeChild(self);
      removed === self; // true
      ```

    + 注意到删除后的节点虽然不在文档树中了，但其实它还在内存中，可以随时再次被添加到别的位置。

    + 当你遍历一个父节点的子节点并进行删除操作时，要注意，`children`属性是一个只读属性，并且它在子节点变化时会实时更新。

      ```javascript
      `<div id="parent">
          <p>First</p>
          <p>Second</p>
      </div>`
      
      var parent = document.getElementById('parent');
      parent.removeChild(parent.children[0]);
      parent.removeChild(parent.children[1]); // <-- 浏览器报错
      ```

    + 浏览器报错：`parent.children[1]`不是一个有效的节点。原因就在于，当`First`节点被删除后，`parent.children`的节点数量已经从2变为了1，索引`[1]`已经不存在了。因此，删除多个节点时，要注意`children`属性时刻都在变化。

+ 操作表单

  + HTML表单的输入控件主要有以下几种：

    ```javascript
    //文本框，对应的<input type="text">，用于输入文本；
    //口令框，对应的<input type="password">，用于输入口令；
    //单选框，对应的<input type="radio">，用于选择一项；
    //复选框，对应的<input type="checkbox">，用于选择多项；
    //下拉框，对应的<select>，用于选择一项；
    //隐藏文本，对应的<input type="hidden">，用户不可见，但表单提交时会把隐藏文本发送到服务器。
    ```

  + 直接调用`value`获得对应的用户输入值,这种方式可以应用于`text`、`password`、`hidden`以及`select`。但是，对于单选框和复选框，`value`属性返回的永远是HTML预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用`checked`判断：

    ```javascript
    // <label><input type="radio" name="weekday" id="monday" value="1"> Monday</label>
    // <label><input type="radio" name="weekday" id="tuesday" value="2"> Tuesday</label>
    var mon = document.getElementById('monday');
    var tue = document.getElementById('tuesday');
    mon.value; // '1'
    tue.value; // '2'
    mon.checked; // true或者false
    tue.checked; // true或者false
    ```

  + 设置值：对于`text`、`password`、`hidden`以及`select`，直接设置`value`就可以；对于单选框和复选框，设置`checked`为`true`或`false`即可。

  + HTML5控件

    + HTML5新增了大量标准控件，常用的包括`date`、`datetime`、`datetime-local`、`color`等，它们都使用``标签：

      ```
      <input type="date" value="2015-07-01">
      ```

    + 不支持HTML5的浏览器无法识别新的控件，会把它们当做`type="text"`来显示。支持HTML5的浏览器将获得格式化的字符串。例如，`type="date"`类型的`input`的`value`将保证是一个有效的`YYYY-MM-DD`格式的日期，或者空字符串。

    + 没有`name`属性的`input`的数据不会被提交。

    + 在检查和修改`input`时，要充分利用`<input type='hidden'>`来传递数据。

      + 例如，很多登录表单希望用户输入用户名和口令，但是，安全考虑，提交表单时不传输明文口令，而是口令的MD5。普通JavaScript开发人员会直接修改`<input>`：

        ```javascript
        <!-- HTML -->
        <form id="login-form" method="post" onsubmit="return checkForm()">
            <input type="text" id="username" name="username">
            <input type="password" id="password" name="password">
            <button type="submit">Submit</button>
        </form>
        
        <script>
        function checkForm() {
            var pwd = document.getElementById('password');
            // 把用户输入的明文变为MD5:
            pwd.value = toMD5(pwd.value);
            // 继续下一步:
            return true;
        }
        </script>
        ```

      + 这个做法看上去没啥问题，但用户输入了口令提交时，口令框的显示会突然从几个`*`变成32个`*`（因为MD5有32个字符）。

        要想不改变用户的输入，可以利用``实现

        ```javascript
        <!-- HTML -->
        <form id="login-form" method="post" onsubmit="return checkForm()">
            <input type="text" id="username" name="username">
            <input type="password" id="input-password">
            <input type="hidden" id="md5-password" name="password">
            <button type="submit">Submit</button>
        </form>
        
        <script>
        function checkForm() {
            var input_pwd = document.getElementById('input-password');
            var md5_pwd = document.getElementById('md5-password');
            // 把用户输入的明文变为MD5:
            md5_pwd.value = toMD5(input_pwd.value);
            // 继续下一步:
            return true;
        }
        </script>
        ```

        

+ 操作文件

  + 在HTML表单中，可以上传文件的唯一控件就是``。
  
  + 当一个表单包含`<input type=file>`时，表单的`enctype`必须指定为`multipart/form-data`，`method`必须指定为`post`，浏览器才能正确编码并以`multipart/form-data`格式发送表单的数据。
    
    + 通常，上传的文件都由后台服务器处理，JavaScript可以在提交表单时对文件扩展名做检查，以便防止用户上传无效格式的文件
    
      ```javascript
      var f = document.getElementById('test-file-upload');
      var filename = f.value; // 'C:\fakepath\test.png'
      if (!filename || !(filename.endsWith('.jpg') || filename.endsWith('.png') || filename.endsWith('.gif'))) {
          alert('Can only upload image file.');
          return false;
      }
      ```
    
  + ### File API
  
    + HTML5的File API提供了`File`和`FileReader`两个主要对象，可以获得文件信息并读取文件
  
    + ```javascript
      var
          fileInput = document.getElementById('test-image-file'),
          info = document.getElementById('test-file-info'),
          preview = document.getElementById('test-image-preview');
      // 监听change事件:
      fileInput.addEventListener('change', function () {
          // 清除背景图片:
          preview.style.backgroundImage = '';
          // 检查文件是否选择:
          if (!fileInput.value) {
              info.innerHTML = '没有选择文件';
              return;
          }
          // 获取File引用:
          var file = fileInput.files[0];
          // 获取File信息:
          info.innerHTML = '文件: ' + file.name + '<br>' +
                           '大小: ' + file.size + '<br>' +
                           '修改: ' + file.lastModifiedDate;
          if (file.type !== 'image/jpeg' && file.type !== 'image/png' && file.type !== 'image/gif') {
              alert('不是有效的图片文件!');
              return;
          }
          // 读取文件:
          var reader = new FileReader();
          reader.onload = function(e) {
              var
                  data = e.target.result; // 'data:image/jpeg;base64,/9j/4AAQSk...(base64编码)...'            
              preview.style.backgroundImage = 'url(' + data + ')';
          };
          // 以DataURL的形式读取文件:
          reader.readAsDataURL(file);
      });
      ```
  
    + 上面的代码还演示了JavaScript的一个重要的特性就是单线程执行模式。在JavaScript中，浏览器的JavaScript执行引擎在执行JavaScript代码时，总是以单线程模式执行，也就是说，任何时候，JavaScript代码都不可能同时有多于1个线程在执行。
  
    + 在JavaScript中，执行多任务实际上都是异步调用，比如上面的代码：
  
      ```javascript
      reader.readAsDataURL(file);
      ```
  
    + 就会发起一个异步操作来读取文件内容。因为是异步操作，所以我们在JavaScript代码中就不知道什么时候操作结束，因此需要先设置一个回调函数：
  
      ```javascript
      reader.onload = function(e) {
          // 当文件读取完成后，自动调用此函数:
      };
      ```
  
    + 当文件读取完成后，JavaScript引擎将自动调用我们设置的回调函数。执行回调函数时，文件已经读取完毕，所以我们可以在回调函数内部安全地获得文件内容。
  
+ AJAX

  + 用JavaScript写一个完整的AJAX代码并不复杂，但是需要注意：AJAX请求是异步执行的，也就是说，要通过回调函数获得响应。在现代浏览器上写AJAX主要依靠`XMLHttpRequest`对象：

    ```javascript
    function success(text) {
        var textarea = document.getElementById('test-response-text');
        textarea.value = text;
    }
    
    function fail(code) {
        var textarea = document.getElementById('test-response-text');
        textarea.value = 'Error code: ' + code;
    }
    
    var request = new XMLHttpRequest(); // 新建XMLHttpRequest对象
    
    request.onreadystatechange = function () { // 状态发生变化时，函数被回调
        if (request.readyState === 4) { // 成功完成
            // 判断响应结果:
            if (request.status === 200) {
                // 成功，通过responseText拿到响应的文本:
                return success(request.responseText);
            } else {
                // 失败，根据响应码判断失败原因:
                return fail(request.status);
            }
        } else {
            // HTTP请求还在继续...
        }
    }
    
    // 发送请求:
    request.open('GET', '/api/categories');
    request.send();
    
    alert('请求已发送，请等待响应...');
    ```

  + 当创建了`XMLHttpRequest`对象后，要先设置`onreadystatechange`的回调函数。在回调函数中，通常我们只需通过`readyState === 4`判断请求是否完成，如果已完成，再根据`status === 200`判断是否是一个成功的响应。

  + `XMLHttpRequest`对象的`open()`方法有3个参数，第一个参数指定是`GET`还是`POST`，第二个参数指定URL地址，第三个参数指定是否使用异步，默认是`true`，所以不用写。

  + `XMLHttpRequest`对象的`open()`方法有3个参数，第一个参数指定是`GET`还是`POST`，第二个参数指定URL地址，第三个参数指定是否使用异步，默认是`true`，所以不用写。注意：千万不要把第三个参数指定为`false`，否则浏览器将停止响应，直到AJAX请求完成。如果这个请求耗时10秒，那么10秒内你会发现浏览器处于“假死”状态。

  + 安全限制

    + 使用flash

    + 同源下架设本地服务器

    + JSONP

      + 有个限制，只能用GET请求，并且要求返回JavaScript。这种方式跨域实际上是利用了浏览器允许跨域引用JavaScript资源：

        ```html
        <html>
        <head>
            <script src="http://example.com/abc.js"></script>
            ...
        </head>
        <body>
        ...
        </body>
        </html>
        ```

      + JSONP通常以函数调用的形式返回，例如，返回JavaScript内容如下：

        ```javascript
        foo('data');
        ```

      + 这样一来，我们如果在页面中先准备好`foo()`函数，然后给页面动态加一个`<script>`节点，相当于动态读取外域的JavaScript资源，最后就等着接收回调了。

      + 示例：

        ```javascript
        //以163的股票查询URL为例，对于URL：http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice，你将得到如下返回：
        refreshPrice({"0000001":{"code": "0000001", ... });
        //因此我们需要首先在页面中准备好回调函数：
        function refreshPrice(data) {
            var p = document.getElementById('test-jsonp');
            p.innerHTML = '当前价格：' +
                data['0000001'].name +': ' + 
                data['0000001'].price + '；' +
                data['1399001'].name + ': ' +
                data['1399001'].price;
        }
        //最后用getPrice()函数触发：
        function getPrice() {
            var
                js = document.createElement('script'),
                head = document.getElementsByTagName('head')[0];
            js.src = 'http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice';
            head.appendChild(js);
        }
        ```

    + CORS

      + 假设本域是`my.com`，外域是`sina.com`，只要响应头`Access-Control-Allow-Origin`为`http://my.com`，或者是`*`，本次请求就可以成功。

      + 对于PUT、DELETE以及其他类型如`application/json`的POST请求，在发送AJAX请求之前，浏览器会先发送一个`OPTIONS`请求（称为preflighted请求）到这个URL上，询问目标服务器是否接受：

        ```javascript
        //OPTIONS请求
        OPTIONS /path/to/resource HTTP/1.1
        Host: bar.com
        Origin: http://my.com
        Access-Control-Request-Method: POST
        
        //服务器必须响应并明确指出允许的Method：
        HTTP/1.1 200 OK
        Access-Control-Allow-Origin: http://my.com
        Access-Control-Allow-Methods: POST, GET, PUT, OPTIONS
        Access-Control-Max-Age: 86400
        
        //浏览器确认服务器响应的Access-Control-Allow-Methods头确实包含将要发送的AJAX请求的Method，才会继续发送AJAX，否则，抛出一个错误。
        //由于以POST、PUT方式传送JSON格式的数据在REST中很常见，所以要跨域正确处理POST和PUT请求，服务器端必须正确响应OPTIONS请求。
        ```

+ Promise(会用，但是不懂原理)

  + 使用Promise实现AJAX：

    ```javascript
    'use strict';
    
    // ajax函数将返回Promise对象:
    function ajax(method, url, data) {
        var request = new XMLHttpRequest();
        return new Promise(function (resolve, reject) {
            request.onreadystatechange = function () {
                if (request.readyState === 4) {
                    if (request.status === 200) {
                        resolve(request.responseText);
                    } else {
                        reject(request.status);
                    }
                }
            };
            request.open(method, url);
            request.send(data);
        });
    }
    //使用
    var log = document.getElementById('test-promise-ajax-result');
    var p = ajax('GET', '/api/categories');
    p.then(function (text) { // 如果AJAX成功，获得响应内容
        log.innerText = text;
    }).catch(function (status) { // 如果AJAX失败，获得响应代码
        log.innerText = 'ERROR: ' + status;
    });
    ```

  + 试想一个页面聊天系统，我们需要从两个不同的URL分别获得用户的个人信息和好友列表，这两个任务是可以并行执行的，用`Promise.all()`实现

    ```javascript
    var p1 = new Promise(function (resolve, reject) {
        setTimeout(resolve, 500, 'P1');
    });
    var p2 = new Promise(function (resolve, reject) {
        setTimeout(resolve, 600, 'P2');
    });
    // 同时执行p1和p2，并在它们都完成后执行then:
    Promise.all([p1, p2]).then(function (results) {
        console.log(results); // 获得一个Array: ['P1', 'P2']
    });
    ```

    

  + 有些时候，多个异步任务是为了容错。比如，同时向两个URL读取用户的个人信息，只需要获得先返回的结果即可。这种情况下，用`Promise.race()`实现

    ```javascript
    var p1 = new Promise(function (resolve, reject) {
        setTimeout(resolve, 500, 'P1');
    });
    var p2 = new Promise(function (resolve, reject) {
        setTimeout(resolve, 600, 'P2');
    });
    Promise.race([p1, p2]).then(function (result) {
        console.log(result); // 'P1'
    });
    ```

    

+ Canvas

  + 一个Canvas定义了一个指定尺寸的矩形框，在这个范围内我们可以随意绘制：

    ```html
    <canvas id="test-canvas" width="300" height="200"></canvas>
    ```

  + 通常在``内部添加一些说明性HTML代码，如果浏览器支持Canvas，它将忽略``内部的HTML，如果浏览器不支持Canvas，它将显示``内部的HTML：

    ```html
    <canvas id="test-stock" width="300" height="200">
        <p>你的浏览器不支持Canvas画板功能</p>
    </canvas>
    ```

  + 在使用Canvas前，可以用`canvas.getContext`来测试浏览器是否支持Canvas

  + 具体使用方法链接（看链接吧，没啥难点，就是得记住用法，本来也都会，就是时间长没用有点忘了）：https://www.liaoxuefeng.com/wiki/1022910821149312/1023022423592576

