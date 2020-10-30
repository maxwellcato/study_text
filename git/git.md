### git学习笔记

#### git的安装

+ 从官网下载git安装程序，然后一路默认安装就可以了

+ 安装成功后需要做的配置工作

  ```shell
  $ git config --global user.name "Your Name"
  $ git config --global user.email "YOUR EMAIL ADDRESS"  # 1507499759@qq.com
  ```

#### git仓库的创建

+ 创建

  ```shell
  $ git init 
  ```

+ 提交代码

  ```shell
  $ git add .
  $ git commit -m 'COMMIT'
  ```

#### 版本管理

+ 版本回退

  ```shell
  $ git status    # 查看仓库当前状态
  $ git diff    #查看仓库内文件的具体修改内容
      #关于git diff命令的补充：
          #git diff比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来(暂存指使用git add命令提交到暂存区)的变化内容。若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --cached 命令。
          #请注意，git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件后，运行 git diff 后却什么也没有，就是这个原因。
          #提交后，用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别(直接使用.表示查看当前目录下的所有文件)
  
  
  $ git log  #git log命令显示从最近到最远的提交日志
  $ git log --pretty=oneline  # 可以将提交日志进行精简，方便查看
  # 在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
  $ git reset --hard HEAD^  #将git回退一个版本
  $ git reset --hard 2e9ccd  #将git回退或前进到某一个版本
  $ git reflog  #该命令可查看每一次的版本更改命令：
  	$ git reflog
      2e9ccdc (HEAD -> master) HEAD@{0}: reset: moving to 2e9ccd
      3b9c27f HEAD@{1}: reset: moving to HEAD~1
      2e9ccdc (HEAD -> master) HEAD@{2}: reset: moving to HEAD~0
      2e9ccdc (HEAD -> master) HEAD@{3}: commit: daily
      3b9c27f HEAD@{4}: commit: test
      fbe8e9d (origin/master) HEAD@{5}: commit (initial): test
  ```

+ 工作区和暂存区

  ```shell
  #名词解释：
  	#工作区：.git隐藏文件所在的那个目录
  	#版本库：.git隐藏目录即为版本库
  	#暂存区：暂存区存在于.git隐藏目录内，被称为stage（或者叫index）的暂存区
  	
  	#Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
  $ git add  'FILE' # 该命令将文件提交到了暂存区
  $git commit -m 'MESSAGE' # 该命令将暂存区的所有内容提交到当前分支
  
  # 创建Git版本库时，Git自动创建了唯一一个master分支
  ```

+ 撤销修改

  ```shell
  $ git checkout -- file  #git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
  	#checkout命令可以丢弃工作区的修改，该命令已不被新版git推荐(依然有效)，官方推荐使用的命令是：
  	$ git restore --file  #如果要全部丢弃，使用.就可以了，两个都是
      #命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
      #一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
      #一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
      #总之，就是让这个文件回到最近一次git commit或git add时的状态
      
  $ git reset HEAD <file>
  # 用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区(工作区的修改不会撤销)
  ```

+ 删除文件

  ```shell
  $ rm test.txt    
  # 删除test.txt文件后
  $ git rm test.txt
  # 然后再提交就可以了
  ```

#### 远程仓库

+ 准备工作

  ```shell
  # 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
  $ ssh-keygen -t rsa -C "youremail@example.com"
  # 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面,然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
  ```

+ 添加远程库

  ```shell
  # 命令：
      $ git remote add origin git@github.com:maxwellcatoo/work_study_log.git
  # 下一步，就可以把本地库的所有内容推送到远程库上：
      $ git push -u origin master
      
      Counting objects: 20, done.
      Delta compression using up to 4 threads.
      Compressing objects: 100% (15/15), done.
      Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
      Total 20 (delta 5), reused 0 (delta 0)
      remote: Resolving deltas: 100% (5/5), done.
      To github.com:michaelliao/learngit.git
       * [new branch]      master -> master
      Branch 'master' set up to track remote branch 'master' from 'origin'.
      
      # 把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
      # 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
  ```

+ 从远程仓库克隆

  ```shell
  # 远程仓库建好的情况下：
      $ git clone git@github.com:michaelliao/gitskills.git
  
      Cloning into 'gitskills'...
      remote: Counting objects: 3, done.
      remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
      Receiving objects: 100% (3/3), done
  
  ```

#### 分支管理(高级用法，很重要)

+ 创建与合并分支

  ```shell
  # 每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
  
  # 当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上
  
  
  # 实际操作：
  	1.# 首先，创建dev分支，然后切换到dev分支：
  	$ git checkout -b dev
  	Switched to a new branch 'dev'
  	# 注意：该命令加上-b后，一条语句相当于执行了以下两条语句
  		$ git branch dev
  		$ git checkout dev
  	2.# 之后，可用git branch命令查看所有分支，当前分支前会有*号
  ```









### git使用时遇到的问题及解决方案

#### 本地仓库损坏，将远程仓库内容下拉到本地

+ 情景描述：

  ```shell
  # 这个的话，之前本地库向远程库push是没有问题的，因为直接把新的文件夹拉到仓库目录中，直接把旧的文件夹给覆盖掉了，然后commit那里就出问题了，死活提交不了。索性直接把本地库删了，然后连接仓库，再使用$git pull命令。总是遇到错误...
  ```

+ 解决方法

  ```shell
  #使用git clone命令，将远程仓库拉到本地，然后，现在本地的仓库已经和远程的仓库连接好了。在本地使用git log命令也可以查看之前每次提交的数据...在本地修改文件的话，直接git add .   git commit -m ''    和git push 一气呵成，就把更新内容提交到远程仓库上了
  ```

+ 解决问题后对问题的分析

  ```shell
  # 最初产生错误的原因：原因感觉是直接用一个修改后了新的文件夹覆盖了之前的旧的文件夹(文件夹里边多了图片，原文件也被修改)，导致无法提交到远程终端
  # git pull失败的原因：因为是在本地新建的git仓库，所以在本地是没有master分支的，这跟远程的仓库分支对应不上，所以使用git pull命令死活拉不下来。    而本地库提交到新创建的远程仓库时，其命令为$ git push -u origin master，加了-u命令，会自动在远程仓库创建master并与本地仓库创建联系，所以可以提交上去。     猜想一下，如果git pull命令以某种方式加上u的话，也许也可以在本地的空白仓库中把远程的仓库内容拉下来。
  ```

  

