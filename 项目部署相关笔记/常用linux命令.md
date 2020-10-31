### 查看系统信息

+ 查看服务器版本信息

  ```shell
  #命令：uname -r,输出为：
  #3.10.0-862.el7.x86_64
  
  #uname -a命令，输出为：(uname -a 和uname --all指令相同，即查看服务器版本的所有信息)
  Linux VM-0-14-centos 3.10.0-862.el7.x86_64 #1 SMP Fri Apr 20 16:44:24 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
  
  
  uname -a 命令下，大部分linux系统输出内容为：'Linux localhost.localdomain 3.10.0-	862.el7.x86_64 #1 SMP Fri Apr 20 16:44:24 UTC 2018 x86_64 x86_64 	x86_64 GNU/Linux'
  输出内容详解：
  # 其中3.10.0-862.el7.x86_64 这个即为内核发行版的信息
  # 命名规则：
  
       	# 主版本号：3
       	# 次版本号：10【奇数为开发版本，偶数为稳定版本】
       	# 修订版本号：0【修改的次数】
       	# 此次版本的第N次修改：862
      	 # el7:redhat enterprise linux 7
      	 # x86_64：编译框架
      	 
  #使用uname --help可查看关羽uname的更多指令
  ```

+ linux系统分类

  + linux系统，主要分debian系和redhat系，还有其它自由的发布版本。

    1、debian系主要有Debian，Ubuntu，Mint等及其衍生版本；

    2、redhat系主要有RedHat，Fedora，CentOs等，

    3、其它有Slackware，Gentoo，Arch linux，LFS，SUSE等。

    4、如果开发用，推荐redhat系，业内公司的服务器多用centos，考虑到平时使用，那么就选择fedora，可以选择最新的发行版。

    5、如果简单用加开发，可以选择debian系，推ubuntu，mint。

    6、如果是技术狂型，那么就推荐Gentoo，Arch linux，LFS，Slackware等。
  
+ ```shell
  系统
  # uname -a               # 查看内核/操作系统/CPU信息
  # head -n 1 /etc/issue   # 查看操作系统版本
  # cat /proc/cpuinfo      # 查看CPU信息
  # hostname               # 查看计算机名
  # lspci -tv              # 列出所有PCI设备
  # lsusb -tv              # 列出所有USB设备
  # lsmod                  # 列出加载的内核模块
  # env                    # 查看环境变量
  #dmidecode | grep "Product Nmae"   #查看服务器型号
  资源
  # free -m                # 查看内存使用量和交换区使用量
  # df -h                  # 查看各分区使用情况
  # du -sh <目录名>        # 查看指定目录的大小
  # grep MemTotal /proc/meminfo   # 查看内存总量
  # grep MemFree /proc/meminfo    # 查看空闲内存量
  # uptime                 # 查看系统运行时间、用户数、负载
  # cat /proc/loadavg      # 查看系统负载
  磁盘和分区
  
  # mount | column -t      # 查看挂接的分区状态
  # fdisk -l               # 查看所有分区
  # swapon -s              # 查看所有交换分区
  # hdparm -i /dev/hda     # 查看磁盘参数(仅适用于IDE设备)
  # dmesg | grep IDE       # 查看启动时IDE设备检测状况
  网络
  
  # ifconfig               # 查看所有网络接口的属性
  # iptables -L            # 查看防火墙设置
  # route -n               # 查看路由表
  # netstat -lntp          # 查看所有监听端口
  # netstat -antp          # 查看所有已经建立的连接
  # netstat -s             # 查看网络统计信息
  进程
  
  # ps -ef                 # 查看所有进程
  # top                    # 实时显示进程状态
  用户
  
  # w                      # 查看活动用户
  # id <用户名>            # 查看指定用户信息
  # last                   # 查看用户登录日志
  # cut -d: -f1 /etc/passwd   # 查看系统所有用户
  # cut -d: -f1 /etc/group    # 查看系统所有组
  # crontab -l             # 查看当前用户的计划任务
  服务
  
  # chkconfig --list       # 列出所有系统服务
  # chkconfig --list | grep on    # 列出所有启动的系统服务
  程序
  
  # rpm -qa                # 查看所有安装的软件包
  ```

  

### 最常见的通用命令

+ ls   查看当前目录下的文件和文件夹

+ vi log.log    使用vim编辑器打开log.log文件

+ centos系统常用文件夹介绍：

  ```shell
  #  /bin    可执行文件
  #  /usr/bin    可执行文件
  #  /sbin    可执行文件
  #  /usr/local/bin 可执行文件
  #  /usr/include 头文件
  #  /usr/local/include    头文件
  #  /lib    库
  #  /usr/lib  库  
  #  /usr/local/lib 库 
  
  #  还有一些配置文件,在/etc下,或者/var下
  
  # 你可以在安装完成后,通过which命令来找对应的 安装后的可执行文件:
  [xx@Archimonde ~]$ which gcc
  /usr/bin/gcc
  ```

  

### 部署nginx时，常用的linux命令有

+ ls  查看当前目录下的文件和文件夹