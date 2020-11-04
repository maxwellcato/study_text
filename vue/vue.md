#####　较杂乱的知识

+ 大小写的问题

  ```js
  // 引用组件的时候，组件的驼峰命名中大写的首字母全部改成'-首字母小写'的形式
  // 另外，组件的属性如果也是多单词连接的话，也用上面的方法来写(这一点尚存疑，是约定俗成的规范吗？我喜欢用_来写，因为这样双击左键可以直接选上整个属性)
  
  //注：看的视频比较老，这个情况好像在高一点的版本的vue中被解除了已经
  ```

+ 模板引入的第二种写法

  ```js
  import NavBar from ''
  compoents: {
  	NavBar
  }
  1. 第一种:
  <nav-bar></nav-bar>
  2. 第二种：
  <p is="nav-bar"></p>
  	//优点
  	可以实现动态引用
  <p :is="nav-bar"></p>
  ```

+ 元素显示和隐藏的过渡效果实现(本质是原元素的逐渐透明和新元素的逐渐显现，这个还可以加移动的效果，功能很强大 )

  ```html
  <div>
  	<transition name="fade">  <!--这个是用来实现过渡效果的vue内部实现的组件-->
  		<p v-show="show">i am show</p>
  	<transition>
  </div>
  <!--补充-->
  ```

  vue实现过渡的两种方式：

  + css

    ![vue中试用css实现过渡效果](.\vue中试用css实现过渡效果.png)
  
    ```
  
    ```

    
  
  + js
  
    ```
    
    ```
  
    

##### computed的一些属性

+ 数据双向绑定时，直接过滤掉number的方法

  ```js
  ...<input v-model="myValue"><p>{{valueWidthNum}}</p>...
  ...data:{
  	myValue: ''
  	}
  	computed:{
  		valueWidthNum(){
  			return this.value.replace(/\d/g/,'')  //重点在于replace中套正则表达式的用法
  		} 
  	}
  ```


##### vue组件之间进行传值

+ props

  ```js
  // 定义传值的类型(可以定义多个，我之前一直以为只能定义一个)
  props:{
  	value: [Number,String]
  }
  
  //关于props的详细用法，写法很多(才知道自己之前了解的只是一小部分)
  // 链接：https://blog.csdn.net/zhao97/article/details/105383657
  ```

##### 自定义属性

##### router-view

```js
// 视图设置

//使用
<router-view name="viewA"></router-view>
<router-view name="viewB"></router-view>
//在routes:[{}...]中进行设置
routers:[
    {
        path: "/apple",
        component: {
            viewA: Apple,   //这里可以对组件进行设置，作用是供使用时通过设置name达到选择不同组件的效果
            viewB: banana
        },
        name: 'applePage',
        children: [
            {
                path: 'red_apple',
                component: RedApple
            }
        ]
    }
]

```



#### --------------------------------------

### 后期零零散散遇到的关于vue的一些问题和知识点

##### 子路由的重定向无法实现的问题

```shell
# vue-router中使用router-link中name属性匹配route配置中的name值来到达到跳转路由时，遇到的对子路由的重定向无法实现
# 解决办法是不使用name的那种连接方式，而是使用直接写路径的那种跳转方式不会发生以上的问题。
```

