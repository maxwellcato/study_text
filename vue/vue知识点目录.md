## 采用五级标题和三级标题交叉的方法来方便识别目录内容，没有包含的意思



### 通过引用cdn来使用vue的方式下，定义并注册组件的方法，以及使用props把父组件中data的数据传给组件的具体用法

+ 官方文档中vue起步中的组件化应用构建



##### 使用 `Object.freeze()`，阻止修改现有的 property(意味着响应系统无法再*追踪*变化)的具体使用方法和作用



### 从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数



##### Class与Style绑定中的自动添加前缀内容的功能理解



### 从 2.3.0 起你可以为 `style` 绑定中的 property 提供一个包含多

##### ---

### 个值的数组，常用于提供多个带前缀的值

+ 这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 `display: flex`。

  ```html
  <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
  ```

##### vue中组件复用的情况(之前我认为是vue为了性能造成的类似于bug一类的牺牲，其实这个利用起来的话也算是提供一个功能)



### 如果我们不需要的上面的那个代码复用的话，可以使用key来进行标记，从而避免，那么问题就是如何使用key进行标记从而防止复用呢？



##### 注意，`v-show` 不支持 `<template>` 元素，也不支持 `v-else`



### v-if和v-show适用的使用场景



##### v-for和v-if同时使用的问题



### 使用v-for循环遍历对象时，本质是按照哪种方法进行遍历的



##### 有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。针对模板标签需要放到table等中出现冲突的情况，的解决方案。



### 从哪三个来源使用模板，上述组件间的限制是不存在的



##### v-on的事件修饰符有哪些，组合顺序对最终效果是否有影响？

+ 补充，2.14新增，v-once在自定义组件中也可以用，但是其他的就只能用到原生的DOM事件起作用
+ 补充，2.30新增，.passive修饰符，可提高移动端性能，注意不要与.prevent一起用



### 2.50新增的.exact修饰符的作用



##### 三个鼠标按键修饰符（2.20新增）

+ ```
  多个复选框，绑定到同一个数组：
  ```

+ ```
  如果 v-model 表达式的初始值未能匹配任何选项，<select> 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。
  ```




### Vue的事件系统分离自浏览器的EventTarget API，尽管它们运行类似，但是$emit和$on不是addEventListener和dispatchEvent的别名



##### 表单输入的修饰符（加在v-model后）

+ .lazy
+ .number
+ .trim



### 使用prop进行传值，在传值的属性没有值的情况下，传的值也是true

+ ```html
  <!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
  <blog-post is-published></blog-post>        <!-- is-published为要传的值 -->
  ```



##### 子组件中最后不要对父组件通过prop传过来的值进行直接修改(之前我连直接修改都不会，后来通过watch监听学会了)，建议:

+ 在data中创建一个属性存prop的值
+ 如果要对prop的格式进行一下修改，建议使用computed方法来进行修改



### vue的异步组件实现方法



##### 在每个 `new Vue` 实例的子组件中，其根实例可以通过 `$root` property 进行访问，以及该方法的适用项目



### 和 `$root` 类似，`$parent` property 可以用来从一个子组件访问父组件的实例



##### $refs` 只会在组件渲染完成之后生效，并且它们不是响应式的。这仅作为一个用于直接操作子组件的“逃生舱”——你应该避免在模板或计算属性中访问 `$refs



### 依赖注入中的`provide` 和 `inject`作用和使用方法



##### 使用$options注册子组件的方法

+ ```js
  beforeCreate: function () {
    this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue').default
  }
  ```



### vue中进行手动强制更新的方法



##### mapGetters的用法（两种）及注意事项



### 使用Mutation更改对象的属性时，不要直接更改，推荐使用的两种方法的具体用法：

+ 使用Vue.set()方法
+ 使用解构赋值的方法state.obj = {...state.obj,'key':'value'}



##### 当有**相同标签名**的元素切换时，需要通过 `key` attribute 设置唯一的值来标记以让 Vue 区分它们



### 半问题半记忆吧，store里module中的getters可以接受四个参数，state、getters、rootState、rootGetters，奇怪的点是getters和rootGetters完全一样，使用了console.log(getters === rootGetters)输出，结果为true（原因是模块的getters中只有那一个我们在其中用getters和rootGetters参数的函数，且未定义命名空间的情况下，getters和rootGetters值是相等的）；rootState中包含有state



##### 命名空间的使用方法和作用



### 在带命名空间的模块注册全局 action的方法



##### 严格模式下Vuex数据绑定v-model的两种实现方法

+ 一种是使用:value和@input结合实现的，这种在标签上用不到v-model
+ 另一种是computed的get方法和set方法来实现,这种在标签上可以用到v-model



### 如果你在 `store.js` 文件中定义了 mutation，并且使用 ES2015 模块功能默认输出了 Vuex.Store 的实例，那么你仍然可以给 mutation 取个变量名然后把它输出去：



##### 同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。



### 值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。`Vue.extend()` 也使用同样的策略进行合并。



##### 