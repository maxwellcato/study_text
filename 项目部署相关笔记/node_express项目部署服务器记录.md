## 主要借鉴文章

+ 链接：https://www.jianshu.com/p/eec8c945f0d3
+ 文章题目：服务器小白的我,是如何将 node+mongodb 项目部署在服务器上并进行性能优化的

### 在服务器上安装node和npm

+ 这个是看菜鸟教程上面的步骤一步一步安装的，最后，查看安装版本也全部成功，以后再弄直接按照菜鸟教程上面的步骤来即可，这个不用记笔记
+ （当时写出上一句的我还不知道这句话犯了多大的错误，菜鸟教程上安装的版本是0.1版本的，而现在都更新到10.2版本了.....后面也因为版本太低导致无法安装pm2，找错误还找了半天。）

### 安装express

+ 之前看的是一篇18年的文章，里边安装`express`的命令是`npm install -g express`，全程安装下来后，会报`-bash: express: command not found`的错误信息，后来经查询得知，这是因为`express`更新的原因，安装`express`新版本的命令已经变了，变成了`npm install -g express-generator`，运行后一个命令，并执行完成后，输入`express --version`即可查看安装版本信息。注：使用`express -v`或`express -V`无法查看信息

+ 补充。不是安装express的命令变了，而是随着express的更新，使用express还需要安装express生成器，

  1. 全局安装express

     ```
     npm install express -g
     ```

  2. 安装express生成器

     ```
     npm install -g express-generator
     ```

  3. 将express设置为全局（这里我没有设置就可以通过使用--version查看express版本）

     ```
     sudo ln -s /data/node-v6.10.1-linux-x64/bin/express/usr/local/bin/express
     ```

  4. 检测是否成功

     ```
     express --version
     ```

     

### 安装mongodb 

+ 不行，我的代码里都是连接的mysql数据库，没法参考这篇文章了，换下一家

## 借鉴文章2

### 安装mysql

+ 链接：https://www.jianshu.com/p/43f71b93e116

+ 安装mysql，`yum install mysql-community-server`

+ 安装完成后，启动mysql服务  systemctl start mysqld

+ 查看mysql的服务是否启动    systemctl status mysqld

  ```shell
  #输出为：
  mysqld.service - MySQL Server
     Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
     Active: active (running) since Sat 2020-10-10 14:53:14 CST; 2min 34s ago
       Docs: man:mysqld(8)
             http://dev.mysql.com/doc/refman/en/using-systemd.html
    Process: 1301 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
    Process: 1284 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
   Main PID: 1304 (mysqld)
     CGroup: /system.slice/mysqld.service
             └─1304 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid
  
  Oct 10 14:53:12 VM-0-14-centos systemd[1]: Starting MySQL Server...
  Oct 10 14:53:14 VM-0-14-centos systemd[1]: Started MySQL Server.
  ```

+ 查看默认生成的密码   cat /var/log/mysqld.log | grep password

  ```shell
  # 该命令没有任何输出(因为这次安装没有设置密码)
  ```

+ 登录mysql 

  ```shell
  #指令： mysql -uroot -p
  #输出：Enter password:
  #直接回车,输出如下信息：
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 3
  Server version: 5.7.31 MySQL Community Server (GPL)
  
  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
  
  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.
  
  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  # 即登录mysql成功，
  ```

+ 修改mysql密码规则

  + | 0 or LOW    | 长度                               |
    | ----------- | ---------------------------------- |
    | 1 or MEDIUM | 长度、大小写、数字、特殊字符       |
    | 2 or STRONG | 长度、大小写、数字、特殊字符、词典 |

  + 详情链接：https://www.jianshu.com/p/56075ccb47ee

+ mysql远程登录设置

  + 首先查询mysql当前用户登录配置

    + 查询语句：

      ```csharp
      select host,user from mysql.user;  //mysql的登录配置在其mysql库的user表中，修改和查询都是通过对这个表中的内容进行修改来进行的。
      ```

      输出结果：

      ```csharp
      +----------------+------+
      | host           | user |
      +----------------+------+
      | 127.0.0.1      | root |
      | ::1            | root |
      | localhost      |      |
      | localhost      | root |
      | vm-0-14-centos |      |
      | vm-0-14-centos | root |
      +----------------+------+
      6 rows in set (0.00 sec)
      //其中，localhost表示只能本机访问
      
      //查询用户名，密码和host
      select host,user,password from mysql.user;
      
      mysql> select host,user,password from user;
      +----------------+------+----------+
      | host           | user | password |
      +----------------+------+----------+
      | localhost      | root |          |
      | vm-0-14-centos | root |          |
      | 127.0.0.1      | root |          |
      | ::1            | root |          |
      | localhost      |      |          |
      | vm-0-14-centos |      |          |
      +----------------+------+----------+
      6 rows in set (0.00 sec)
      ```

  + 远程登录设置

    + 设置语句： 

      ```csharp
      update user set host='%' where user = 'root';
      会报错，可能是因为我的数据库表中有多个root的缘故，而参考案例中人家的root只有一个
      //报错信息：
      ERROR 1062 (23000): Duplicate entry '%-root' for key 'PRIMARY'
      //虽然有报错信息，但是数据库中的第一行信息确实被修改了
      //再次查询发现：
      +----------------+------+----------+
      | host           | user | password |
      +----------------+------+----------+
      | %              | root |          |
      | vm-0-14-centos | root |          |
      | 127.0.0.1      | root |          |
      | ::1            | root |          |
      | localhost      |      |          |
      | vm-0-14-centos |      |          |
      +----------------+------+----------+
      6 rows in set (0.00 sec)
      //这样的话，远程配置应该算完成了
      
      //刷新缓存（更改后用的，我的似乎并不需要用，更改后查询就发现已经改了，而且后来使用刷新缓存语句时，系统提示也是更改了0行数据）
      flush privileges;
      ```

  + 因为使用本机的cat软件远程连接服务器数据库需要输入数据库端口号，所以，需要查询服务器上的数据库端口号

    + 查询指令：

  + 倒腾了好久，因为修改问题（？）或者其他啥问题，导致我登录上mysql时，不显示mysql库和其他一些库，只剩下了一个test库和一个information_schema库，网上查了一下，好像是因为存在空用户名，导致每次登录都是默认使用的那个空用户名登录的，而那个用户是没有什么权限的，所以不管用那个账号登录都没办法显示mysql库，更没法访问。而解决空用户名的问题则必须修改mysql库中的user表。

  + 解决办法，打开my.inf文件，Linux下MySQL的配置文件是my.cnf,一般会放在/etc/my.cnf,/etc/mysql/my.cnf，在［mysqld］下添加：
    `skip-grant-tables`,保存文件，重启mysql服务，即可登录查看到mysql数据库，修改好文件后再把添加内容注释掉即可。

### 安装pm2

+ npm install -g pm2

+ 这里死活安装不上来，后来查了半天才发现是node版本太低了，0.10的版本.......，之前我还以为linux上的版本表示方法跟windows不一样，看来...后来捣鼓半天，还没法用npm给node升级，早知道就不该参考菜鸟驿站的教程来安装node的

  + 后来，将node和npm全部卸载，又重装了，这次重装体验非常好，参考的是简书上面的一些文章，看来以后尽量不要在百度和csdn上查问题了，很容易出问题。文章参考链接：https://www.jianshu.com/p/99a3d43c7988，先安装了git，然后安装nvm（node版本管理工具），然后直接nvm命令安装指定版本的node就行了。

  + 安装pm2应该算是成功了，但是因为全局变量的问题，需要创建一个软连接，软连接命令：

    ```shell
    #软连接命令：ln -s /root/node-v12.14.0-linux-x64/bin/pm2 /usr/local/bin/
    #命令解析：ln -s 'pm安装路径'
    #根据安装pm成功所显示的系统信息，拿到pm安装路径，
    #/root/.nvm/versions/node/v12.18.3/bin/
    
    #安装pm2指令：
    [root@VM-0-14-centos ~]# npm install -g pm2
    # 输出信息：
    /root/.nvm/versions/node/v12.18.3/bin/pm2 -> /root/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2
    /root/.nvm/versions/node/v12.18.3/bin/pm2-dev -> /root/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2-dev
    /root/.nvm/versions/node/v12.18.3/bin/pm2-docker -> /root/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2-docker
    /root/.nvm/versions/node/v12.18.3/bin/pm2-runtime -> /root/.nvm/versions/node/v12.18.3/lib/node_modules/pm2/bin/pm2-runtime
    npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.1.2 (node_modules/pm2/node_modules/chokidar/node_modules/fsevents):
    npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.1.3: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
    
    + pm2@4.5.0
    added 192 packages from 191 contributors in 69.149s
    ```

### pm2设置

+ 设置pm2开机启动

  ```cpp
  pm2 startup centos
  ```

+ 在自己的node项目目录下创建一个processes.json文件
  
  ```cpp
  {
       "apps": [
           {
               "name": "xxx", //自己定义好的进程标识,到时候通过此标识查看是哪个进程
               "script": "./bin/www", //pm2启动的程序目录
               "log_date_format": "YYYY-MM-DD HH:mm Z",
               "log_file"   : "./logs/log_file.log",//nodejs输出的后台日志
               "error_file" : "./logs/error_file.log",//当后台有错误时输出的日志
               "out_file"   : "./logs/out_file.log",//log_file和error_file合并输出的日志
               "merge_logs": true,
           }
       ]
   }
  ```
  
+ 启动项目

  ```cpp
  pm2 start processes.json
  //这样后端项目就已经挂载好了
  //重新加载项目项目：
  pm2 reload all
  ```
### pm2常用命令

```shell
1. 启动

# pm2 start app.js
# pm2 start app.js --name my-api   #my-api为PM2进程名称
# pm2 start app.js -i 0           #根据CPU核数启动进程个数
# pm2 start app.js --watch   #实时监控app.js的方式启动，当app.js文件有变动时，pm2会自动reload
2. 查看进程

# pm2 list
# pm2 show 0 或者 # pm2 info 0  #查看进程详细信息，0为PM2进程id
3. 监控

# pm2 monit
4. 停止

# pm2 stop all  #停止PM2列表中所有的进程
# pm2 stop 0    #停止PM2列表中进程为0的进程
5. 重载

# pm2 reload all    #重载PM2列表中所有的进程
# pm2 reload 0     #重载PM2列表中进程为0的进程
6. 重启

# pm2 restart all     #重启PM2列表中所有的进程
# pm2 restart 0      #重启PM2列表中进程为0的进程
7. 删除PM2进程

# pm2 delete 0     #删除PM2列表中进程为0的进程
# pm2 delete all   #删除PM2列表中所有的进程
8. 日志操作

# pm2 logs [--raw]   #Display all processes logs in streaming
# pm2 flush              #Empty all log file
# pm2 reloadLogs    #Reload all logs
9. 升级PM2

# npm install pm2@lastest -g   #安装最新的PM2版本
# pm2 updatePM2                    #升级pm2
10. 更多命令参数请查看帮助

# pm2 --help
  
# 参考链接 : https://blog.csdn.net/taoerchun/article/details/81537654
(参考文章还有其它内容，写的比较乱，就不妨到这里了，需要查看的话看网页链接吧)
```