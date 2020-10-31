栅格的最后一部分内容less和mixin等会回去看一下，没看懂
##### 在一个新的文件中使用`npm install bootstrap`命令安装bootstrap报错：缺乏package.json文件

+ 解决办法：使用`npm init`创建一个package.json包
+ 使用`npm init`命令需要勾选一系列内容，如果这些内容默认就可以，可以直接运行`npm init --yes`

##### 按照上述方法全选默认操作时，会报如下错误：
+ 报错信息：
	```
	Refusing to install package with name "bootstrap" under a package also called "bootstrap". Did you name your project the same as the dependency you're installing?```
+ 这是因为要安装的包的名字和package.json的名字冲突了，打开package.json，修改第一行的name即可。

##### 布局相关类名

+ container-fluid
+ container

##### 栅格系统

+ `row`的含义及其应该放在哪些类中才能起作用
+ `row`的嵌套使用
+ `col-push-3`的本质应该是向后移动3个栅格的距离，如果后边紧挨着存在栅格，则其会被后边的栅格覆盖；如果后边是空的，那么这个类的效果跟偏移的效果时一样的。
+ `col-pull-3`同理，向前移动三个栅格的距离，如果前边有栅格，则把前面的栅格覆盖掉，一个栅格被覆盖还是覆盖其他栅格，由其位置决定。后边的栅格总是覆盖前边的栅格

##### 排版

+ lead可以让段落突出显示，还会增加元素的margin-bottom

##### 表格

+ 使用table-striped使表格变成条纹状表格时，必须要有类名table，否则无法生效