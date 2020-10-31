# express的安装

+ 直接使用npm进行安装

```javascript
npm install express --save		//安装到本地
npm install express -g		//安装到全局
//此时使用express -v查看版本信息会报错，还需要安装express-generator脚手架才可以，这个脚手架会自动安装一些包方便我们使用express
npm install express-generator -g		//此时使用express --version可以正确弹出版本名称
```

# express的使用

#### 前言

ndoe.js，一个基于javsscript运行环境的服务器语言，它的出现使得javascript有能力去实现服务器操作。在gitHub上ndoe.js的star数已接近6万，可见其受欢迎程度；而基于node.js的Express则把原先的许多操作变的简单灵活，一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。使用 Express 可以快速地搭建一个完整功能的网站。

express官方网址：[www.expressjs.com.cn](http://www.expressjs.com.cn)

##### Express的安装方式

Express的安装可直接使用npm包管理器上的项目，在安装npm之前可先安装淘宝镜像：

```javascript
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

这样我们使用cnpm的来代替npm，这使得下载速度提高很多；其次你需要在你项目目录下运行以下指令来初始化npm，期间所有提示按enter键即可，这会生成package.json，它是用于描述项目文件的。

```javascript
cnpm init
```

再输入

```javascript
cnpm install
```

这下项目目录中又会多出一个叫node_modules文件夹，里面是node.js为我们提供的模块，当然现在没有。接下来便是真正的安装express了，执行：

```javascript
cnpm install express --save
```

这时，我们看到node_modules文件夹多了许多不同版本的应用文件夹，接下来执行

```javascript
express --version
```

查看express是否安装成功，如果显示版本号，则安装正确。

#### Express脚手架的安装

安装Express脚手架有两种方式：

##### 1、使用express-generator安装

使用命令行进入项目目录，依次执行：

```javascript
cnpm i express-generator
```

可通过express -h查看命令行的指令含义

```javascript
express -h
```

 

```javascript
Usage: express [options] [dir]
```

​    

```javascript
Options:
    --version        输出版本号
-e, --ejs            添加对 ejs 模板引擎的支持
    --pug            添加对 pug 模板引擎的支持
    --hbs            添加对 handlebars 模板引擎的支持
-H, --hogan          添加对 hogan.js 模板引擎的支持
-v, --view <engine>  添加对视图引擎（view） <engine> 的支持 (ejs|hbs|hjs|jade|pug|twig|vash) （默认是 jade 模板引擎）
    --no-view        创建不带视图引擎的项目
-c, --css <engine>   添加样式表引擎 <engine> 的支持 (less|stylus|compass|sass) （默认是普通的 css 文件）
    --git            添加 .gitignore
-f, --force          强制在非空目录下创建
-h, --help           输出使用方法
```

创建了一个名为 myapp 的 Express 应用，并使用ejs模板引擎

```javascript
express --view=ejs app   //等号前后不要随意加空格，加了会导致错误
```

进入app，并安装依赖

```javascript
cd myapp
npm install
```

**在Windows 下，使用以下命令启Express应用：**

```javascript
set DEBUG=app:* & npm start
```

**在 MacOS 或 Linux 下，使用以下命令启Express应用：**

```javascript
DEBUG=app:* npm start
```

 

##### 2、使用 express 命令 来快速从创建一个项目目录

express 项目文件夹的名字 -e 如 使用命令行进入项目目录，依次执行：

```javascript
express app -e
cd app
cnpm install
```

 

这时，你也可以看到在app文件夹下的文件结构；

```javascript
bin: 启动目录 里面包含了一个启动文件 www 默认监听端口是 3000 (直接node www执行即可)
node_modules：依赖的模块包
public：存放静态资源
routes：路由操作
views：存放ejs模板引擎
app.js：主文件
package.json：项目描述文件
```

这时，你也可以看到在app文件夹下的文件结构； bin: 启动目录 里面包含了一个启动文件 www 默认监听端口是 3000 (直接node www执行即可) node_modules：依赖的模块包 public：存放静态资源 routes：路由操作 views：存放ejs模板引擎 app.js：主文件 package.json：项目描述文件

 

#### 第一个Express应用“Hello World”

在这里，我们不使用npm构建的脚手架，而是向最开始那样直接在主目录中新建一个app.js文件。

在app.js中输入

```javascript
const express = require('express');     //引入express模块
var app= express();     //express()是express模块顶级函数

app.get('/',function(req,res){      //访问根路径时输出hello world
    res.send(`<h1 style='color: blue'>hello world</h1>`);
});

app.listen(8080);       //设置访问端口号

```

命令行进入项目文件夹后，键入

```javascript
node app.js
```

即已开启服务器，接下来只需在浏览器中运行 http://localhost:8080/ 就可以访问到服务器得到响应后的数据



# Express路由

### 一、Express路由简介

路由表示应用程序端点 (URI) 的定义以及响应客户端请求的方式。它包含一个请求方时（methods）、路径（path）和路由匹配时的函数（callback）;

```
app.methods(path, callback);
```

### 二、Express路由方法

Express方法源于 HTTP 方法之一，附加到 express 类的实例。它可请求的方法包括：

get、post、put、head、delete、options、trace、copy、lock、mkcol、move、purge、propfind、proppatch、unlock、report、mkactivity、checkout、merge、m-search、notify、subscribe、unsubscribe、patch、search 和 connect。

### 三、路径

Express路径包含三种表达形式，分别为字符串、字符串模式、正则表达式

#### 1.字符串路径

```
app.get("/login",function(req,res){
    res.send("heng... women");
})
```

此路径地址将与/login匹配

#### 2.字符串模式路径

此路由路径将与`acd`和相匹配`abcd`。

```
app.get('/ab?cd', function (req, res) {
  res.send('ab?cd')
})
```

这条路线的路径将会匹配`abcd`，`abbcd`，`abbbcd`，等等。

```
app.get('/ab+cd', function (req, res) {
  res.send('ab+cd')
})
```

这条路线的路径将会匹配`abcd`，`abxcd`，`abRANDOMcd`，`ab123cd`，等。

```
app.get('/ab*cd', function (req, res) {
  res.send('ab*cd')
})
```

此路由路径将与`/abe`和相匹配`/abcde`。

```
app.get('/ab(cd)?e', function (req, res) {
  res.send('ab(cd)?e')
})
```

#### 3.正则表达式路径

此路由路径将匹配其中带有“ a”的任何内容。

```
app.get(/a/, function (req, res) {
  res.send('/a/')
})
```

这条路线的路径将匹配`butterfly`和`dragonfly`，但不`butterflyman`，`dragonflyman`等。

```
app.get(/.*fly$/, function (req, res) {
  res.send('/.*fly$/')
})
```

\#### 

### 四、基础路由

```
const express = require("express");
var app = express();

app.get("/",function(req,res){
    res.send(`<h1>主页</h1>`);
});
app.get("/login",function(req,res){
    res.send(“登录页面”);
});
app.get("/registe",function(req,res){
    res.send(“注册页面”);
});

app.listen(8080);
```

输入http://127.0.0.1:8080/login和http://127.0.0.1:8080/registe都能进入不同路由。

### 五、动态路由

#### 路线参数

路由参数被命名为URL段，用于捕获URL中在其位置处指定的值。捕获的值将填充到`req.params`对象中，并将路径中指定的route参数的名称作为其各自的键。

```
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

要使用路由参数定义路由，只需在路由路径中指定路由参数，如下所示。

```
app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
})
```

路径参数的名称必须由“文字字符”（[A-Za-z0-9_]）组成。

由于连字符（`-`）和点（`.`）是按字面解释的，因此可以将它们与路由参数一起使用，以实现有用的目的。

```
Route path: /flights/:from-:to
Request URL: http://localhost:3000/flights/LAX-SFO
req.params: { "from": "LAX", "to": "SFO" }
Route path: /plantae/:genus.:species
Request URL: http://localhost:3000/plantae/Prunus.persica
req.params: { "genus": "Prunus", "species": "persica" }
```

要更好地控制可以由route参数匹配的确切字符串，可以在括号（`()`）后面附加一个正则表达式：

```
Route path: /user/:userId(\d+)
Request URL: http://localhost:3000/user/42
req.params: {"userId": "42"}
```

由于正则表达式通常是文字字符串的一部分，因此请确保`\`使用其他反斜杠对所有字符进行转义，例如`\\d+`。

在Express 4.x中，[不以常规方式解释正则表达式中](https://github.com/expressjs/express/issues/2495)[的`*`字符](https://github.com/expressjs/express/issues/2495)。解决方法是使用`{0,}`代替`*`。这可能会在Express 5中修复。

#### 路线处理程序

您可以提供行为类似于[中间件的](http://www.expressjs.com.cn/en/guide/using-middleware.html)多个回调函数来处理请求。唯一的例外是这些回调可能会调用`next('route')`以绕过其余的路由回调。您可以使用此机制在路由上施加先决条件，然后在没有理由继续使用当前路由的情况下将控制权传递给后续路由。

路由处理程序可以采用函数，函数数组或二者组合的形式，如以下示例所示。

单个回调函数可以处理路由。例如：

```
app.get('/example/a', function (req, res) {
  res.send('Hello from A!')
})
```

多个回调函数可以处理一条路由（确保指定了`next`对象）。例如：

```
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from B!')
})
```

回调函数数组可以处理路由。例如：

```
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

var cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])
```

独立功能和功能数组的组合可以处理路由。例如：

```
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from D!')
})
```

#### 应对方法

`res`下表中响应对象（）上的方法可以将响应发送到客户端，并终止请求-响应周期。如果从路由处理程序中未调用这些方法，则客户端请求将被挂起。

| 方法                                                         | 描述                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [res.download()](http://www.expressjs.com.cn/en/4x/api.html#res.download) | 提示要下载的文件。                                     |
| [res.end（）](http://www.expressjs.com.cn/en/4x/api.html#res.end) | 结束响应过程。                                         |
| [res.json（）](http://www.expressjs.com.cn/en/4x/api.html#res.json) | 发送JSON响应。                                         |
| [res.jsonp（）](http://www.expressjs.com.cn/en/4x/api.html#res.jsonp) | 发送带有JSONP支持的JSON响应。                          |
| [res.redirect（）](http://www.expressjs.com.cn/en/4x/api.html#res.redirect) | 重定向请求。                                           |
| [res.render（）](http://www.expressjs.com.cn/en/4x/api.html#res.render) | 渲染视图模板。                                         |
| [res.send（）](http://www.expressjs.com.cn/en/4x/api.html#res.send) | 发送各种类型的响应。                                   |
| [res.sendFile（）](http://www.expressjs.com.cn/en/4x/api.html#res.sendFile) | 将文件作为八位字节流发送。                             |
| [res.sendStatus（）](http://www.expressjs.com.cn/en/4x/api.html#res.sendStatus) | 设置响应状态代码，并将其字符串表示形式发送为响应正文。 |

#### app.route（）

您可以使用来为路由路径创建可链接的路由处理程序`app.route()`。由于路径是在单个位置指定的，因此创建模块化路由非常有帮助，减少冗余和错别字也很有帮助。有关路由的更多信息，请参见：[Router（）文档](http://www.expressjs.com.cn/en/4x/api.html#router)。

这是使用定义的链式路由处理程序的示例`app.route()`。

```
app.route('/book')
  .get(function (req, res) {
    res.send('Get a random book')
  })
  .post(function (req, res) {
    res.send('Add a book')
  })
  .put(function (req, res) {
    res.send('Update the book')
  })
```

#### 快速路由器

使用`express.Router`该类创建模块化的，可安装的路由处理程序。一个`Router`实例是一个完整的中间件和路由系统; 因此，它通常被称为“迷你应用程序”。

以下示例将路由器创建为模块，在其中加载中间件功能，定义一些路由，并将路由器模块安装在主应用程序的路径上。

`birds.js`在app目录中创建一个名为以下内容的路由器文件：

```
var express = require('express')
var router = express.Router()

// middleware that is specific to this router
router.use(function timeLog (req, res, next) {
  console.log('Time: ', Date.now())
  next()
})
// define the home page route
router.get('/', function (req, res) {
  res.send('Birds home page')
})
// define the about route
router.get('/about', function (req, res) {
  res.send('About birds')
})

module.exports = router
```

然后，在应用程序中加载路由器模块：

```
var birds = require('./birds')
// ...
app.use('/birds', birds)
```

该应用程序现在将能够处理对`/birds`和的请求`/birds/about`，以及调用`timeLog`特定于该路线的中间件功能。

 

# EJS模板

### 一、简介

相比于jade模板引擎，ejs对原HTML语言就未作出结构上的改变，只不过在其交互数据方面做出了些许修改，相比于jade更加简单易用。因此其学习成本是很低的。您也可参考ejs官网：https://ejs.bootcss.com/

### 二、ejs基本使用

这里我们使用如下配置文件：

我们啊可以通过下面的方式实现基本的ejs操作： app.js文件：

```
const express=require("express");
const ejs=require("ejs");
const fs=require("fs");

var app=express();

//引用ejs
app.set('views',"public");  //设置视图的对应目录
app.set("view engine","ejs");       //设置默认的模板引擎
app.engine('ejs', ejs.__express);       //定义模板引擎

app.get("/",function(req,res){
    res.render("index.ejs",{title: "<h4>express</h4>"});
});

app.listen(8080);
```

ejs文件：

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <% for(var i=0;i<10;i++){ %>
            <%= i %>
        <% } %>
        <!-- 获取变量 -->
        <div class="datas">
            <p>获取变量：</p>
            <%- title %>
            <%= title %>
        </div>
    </body>
</html>
```

这时我们会得到如下图的结果：

由此可以知道：

```
<% xxx %>：里面写入的是js语法，
<%= xxx %>：里面是服务端发送给ejs模板转义后的变量，输出为原html
<%- xxx %>：里面也是服务端发送给ejs模板后的变量，不过他会把html输出来
<%# 注释标签，不执行、不输出内容
```

同理res.render()函数也是支持回调的：

```
res.render('user', { name: 'Tobi' }, function(err, html) {
  console.log(html);
});
```

这样我们即可将看到heml的内容。另外值得说明的是ejs模块也有ejs.render()和ejs.renderFile()方法，他在这里与res.render()作用类似，如下：

```
ejs.render(str, data, options);

ejs.renderFile(filename, data, options, function(err, str){
    // str => 输出绘制后的 HTML
});
```

### 三、ejs标签各种含义

```
<% '脚本' 标签，用于流程控制，无输出。
<%_ 删除其前面的空格符
<%= 输出数据到模板（输出是转义 HTML 标签）
<%- 输出非转义的数据到模板
<%# 注释标签，不执行、不输出内容
<%% 输出字符串 '<%'
%> 一般结束标签
-%> 删除紧随其后的换行符
_%> 将结束标签后面的空格符删除
```

以上就为ejs基本用法，往后对数据库操作就直接把json数据从服务器返送给模板引擎就行；



# 总结

后台：分析请求，响应

express就是快速能让我完成这样的后端服务；

实例化应用

```
let app = express()
```

请求和响应规律由路由决定

```
app.get('/',(req,res)=>{
	res.send('html内容')//res.send可以直接发送HTML
})
```

动态路由

```
app.get('/books/:bookid',(req,res)=>{
	console.log(req.params.bookid)
})
```

响应由模板进行渲染

```
res.render('模板路径',模板里需要用到的变量)
```

热重启

```
nodemon .bin/www
```

