没写完，把这两篇文章参考完就差不多好了，很晚了，明天再写

https://www.jianshu.com/p/73d9d5d2d604

https://blog.csdn.net/weixin_49592546/article/details/109462462

### 通用的插件

#### 建议必备

+ Auto Close Tag 和 Auto Rename Tag

  ```
  第一个是写头标签回车自动尾标签补齐，第二个是改头标签尾标签跟着改
  在修改标签名时，能在你修改开始（结束）标签的时候修改对应的结束（开始）标签，帮你减少 50% 的击键；
  
  还有一个Auto Complete Tag —  结合自动重命名和自动闭合标记的功能。(安装试了一下，发现没有自动重命名的功能啊),还是用上边那俩吧
  ```

+ TODO Tree（标记代码）

  ```
  对需要修改的代码做标记，方便修改完另一处后快速定位，支持高亮和创建标签；竞争对手是TODO Heightlight（这个好像只能实现高亮）
  ```

+ Bracket Pair Colorizer（括号显示不同颜色）

  ```
  Bracket Pair Colorizer这款插件可以给()、[]、{}这些常用括号显示不同颜色，当点击对应括号时能够用线段直接链接到一起，让层次结构一目了然。除此之外，它还支持用户自定义符号。
  
  该插件已经出了Bracket Pair Colorizer 2，不过感觉两者功能其实差不多，而且使用这个也就图他的括号显示不同颜色的功能，两个版本在这点上差不多。
  ```

+ Eslint（代码规范）

  ```
  代码规范超严格，自动检查代码使用不规范(不是错误，是不规范)的地方，建议使用(用习惯了就好了)
  ```

+ Beautify

  ```
  Beautify是格式化代码的插件
  可美化JS、JSON、CSS、Sass、HTML（其他类型的文件不行）
  在文件夹根目录下创建 .jsbeautifyrc 文件
  详细介绍链接：https://blog.csdn.net/zwli96/article/details/86543130
  ```

+ snippets（代码片段）

  ```
  自定义代码片段提示，有很多种。常用的有：
  html snippets
  JavaScript (ES6) code snippets
  React-Native/React/Redux snippets for es6/es7  (还不会react,先不安)
  React Standard Style code snippets  (还不会react,先不安)
  ```

  

#### 非必备（选择性安装）

+ Better Align（对齐代码）

  ```
  Better Align主要用于代码的上下对齐，它能够用冒号（：）、赋值（=，+=，-=，*=，/=）和箭头（=>）对齐代码。使用方法：Ctrl+Shift+p输入“Align”确认即可。
  ```

+ local history（保存备份）

  ```
  安装这款插件之后在侧边栏会出现LOCAL HISTORY的字样，每当我们保存更改时，它都会备份一份历史文件，当我们需要恢复之前版本时，只需要点击一下对应的文件即可。此外，它还会在编辑框显示对比详情，能够让你对修改位置一目了然。
  ```

+ Codelf（智能命名提示）

  ```
  通过搜索GitHub, Bitbucket, GitLab来找到真实的使用变量名，为你提供一些高频使用的词汇，同时为你标明使用的语言、代码链接,总之，智能提示命名
  ```

+ change-case（快速修改命名）

  ```
  一款快速修改当前选定内容或当前单词的命名的插件。（从驼峰快速改成_连接的形式等）
  ```

+ Partial Diff（比较文本差异）

  ```
  比较两段代码、输出日志信息的差别
  ```

+ vscode-icons（图标）

  ```
  用好看的图标展示文件夹和文件，而且可以自动检测项目，根据项目不同功能配上不同图标，例如，git、Markdown、配置项、工具类等等
  ```

+ Better Comments（注释）

  ```
  Better Comments这款插件可以让VS Code注释信息更加人性化，它可以根据告警、查询、TODO、高亮等标记对注释进行不同的展示。此外，还可以对注释掉的代码进行样式设置，任何其他注释样式都可以在设置中指定
  ```

+ Markdown All in One（markdown功能实现）

  ```
  这款插件可以实现媲美Typora的Markdown编辑体验
  ```

+ Import Cost

  ```
  该扩展允许您查看导入模块的大小，它对 Webpack 中的 bundlers 有很大帮助，你可以查看是导入整个库还是只导入特定的实用程序。
  ```

  

### vue相关插件
#### 建议必备
+ vetur（必备的东西没啥好说的，vue必需）

  ```
  想要编辑器识别vue文件需要安装vue插件,在VSCode上好用的是vetur
  ```
#### 非必备