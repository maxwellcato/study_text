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
  $ git reflog  #该命令可查看每一次的版本更改命令:
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
  	2.# 之后，可用git branch命令查看所有分支，当前分支*(此时应该是dev分支)前会有*号，此时修改文件内容并提交的话，将被提交到dev分支上。
  		$ git branch
  	3.# git checkout master  切回到master分支，此时查看文件内容，会发现内容回退了，因为之前的提交提交到了dev分支上。
  		$ git checkout master
  	4.# git merge dev   merge命令将把dev分支的内容合并到当前分支上(在这里是master分支)
  		$ git merge dev
  	5.# 合并之后，删除dev分支的命令：
  		$ git branch -d dev    # 这样就把dev分支删除掉了
  	
  # 注注注: 切换分支命令git checkout dev和撤销修改命令git checkout --<file>都用的checkout,所以新版的git推荐使用switch替换掉checkout(替换后创建+切换分支命令的-b需要换成-c，切换分支直接改就好了)
  # 小结：
  Git鼓励大量使用分支：
  查看分支：git branch
  创建分支：git branch <name>
  切换分支：git checkout <name>或者git switch <name>
  创建+切换分支：git checkout -b <name>或者git switch -c <name>
  合并某分支到当前分支：git merge <name>
  删除分支：git branch -d <name>
  ```
  
+ 解决冲突

  ```shell
  # 冲突产生的原因：
  	#两个分支在交点之后，均进行了新的独立的提交操作，这样回导致分支合并失败
          $ git merge dev
          Auto-merging text.txt
          CONFLICT (content): Merge conflict in text.txt
          Automatic merge failed; fix conflicts and then commit the result.
      # 提示合并发生了冲突。同时冲突信息会被写到冲突文件中(可使用git status查看冲突文件):
          <<<<<<< HEAD
          检验冲突  master分支
          =======
          检验冲突dev
          >>>>>>> dev
  	# 对冲突文件手动进行修改，然后保存提交。再删除dev分支.(因为当前在master分支上，所以手动修改后再提交，master就是正确修改版本，dev的就可以删除了)
  	
  	
  	# 注: 带参数的git log可以查看分支的合并情况
  	$git log --graph   # 此命令可以看到分支合并图。
  	$ git log --graph --pretty=oneline --abbrev-commit   # 精简模式
  	# 其中，--pretty=oneline会把提交记录在一行显示，精简掉每次提交的author和data
  	# --abbrev-commit则会将版本号精简到前
  ```

+ 分支管理策略

  ```shell
  # 上边的分支合并默认是使用Fast forward模式，这种模式下，删除分支后，会彻底丢失掉分支信息
  # 而禁用掉该模式时，系统会在merge合并时生成一个新的commit，然后我们就可以从分支历史上看到分支信息
  
  $ git merge --no-ff -m "merge with no-ff" dev
  # 注意--no-ff参数，表示禁用Fast forward
  # 然后，因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
  ```

  分支管理

  ```shell
  # 在团队开发中，应该使用一下几种原则进行分支管理:
  1.# master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活
  2.# 大家都把东西提交到dev上，每个人都要在自己的分支上干活
  ```

+ bug分支

  ```shell
  # 在开发中，bug通过来创建临时分支来修复，修复好后合并分支，并把临时分支删除
  # 应用场景:
  	# (当前在dev分支)代码有bug，所以没有办法提交(所以就没有办法保存)，如果创建分支用来修改bug就会导致当前未保存内容丢失，此时:
  	# 使用Git的stash功能，把当前工作现场“储藏”起来，等以后恢复现场后继续工作
  	$ git stash    # 是的，你没有看错，就两个单词，这么写没错
  	Saved working directory and index state WIP on dev: f52c633 add merge
  # 然后，切换到有bug的分支，再在此创建一个用来修复bug的分支，修改好后提交，合并分支到master上，然后把bug分支删除(建议使用--no-ff)
  # 修改好后，回到dev继续干活，然后把暂存区的内容恢复(这里使用git stash list可以查看暂存区内容):
  	1.一是用git stash apply恢复，但是恢复后，stash内容并不删除，还需要用git stash drop来删除
  	2.另一种方式是用git stash pop，恢复的同时把stash内容也删了
  	
  	
  # 上述方法，把master分支的bug修复了，但是当前工作的dev分支依然没有修复，所以我们需要使用再把dev上的文件按照之前修改bug的方式再修复一次。    所以下边是针对这种重复改bug的改进版方案:
  	# 同样的bug，要在dev上修复，我们只需要把4c805e2 fix bug 101这个提交所做的修改“复制”到dev分支。注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。
  	# git提供了cherry-pick命令，让我们可以复制一个特定的提交到当前分支(这个命令就相当于在当前分支像复制的特定提交一样修改内容并提交):
  	$ git branch
      * dev
        master
      $ git cherry-pick 4c805e2
      [master 1d4b803] fix bug 101
       1 file changed, 1 insertion(+), 1 deletion(-)
       # 这样就借助于其他分支的代码提交修改了本分支的代码提交
       
   # git stash命令补充:
   	1.# 首先，我们修改文件内容，不管是否git add,之后我们git stash。此时，工作区的文件会回退到本句的'首先'那里去。然后，我们把暂存区的内容取出来，文件内容会变成修改之后但git add之前的，不管你之前是否进行了git add操作。
   	2.# 注意，当我们修改文件，stash后。再修改文件，然后commit。如果此时执行git stash pop命令将会产生冲突(跟两个分支分别提交然后合并时的冲突是一样的，即冲突文件中出现两个修改的内容):
   		<<<<<<< Updated upstream
          git stash命令练习2
          =======
          git stash命令练习
          >>>>>>> Stashed changes
          # 所以，我觉着stash其实就是系统新建的分支？只不过不能像平常分支的操作一样，只能查看list啥的。
        # 我们修改文件提交后，stash list中依然存有stash的信息(这个也有需要注意的点，即我们使用git stash pop命令取的stash中的内容,这个指令如果成功删除stash信息，如果未成功，则不会删除)，我们继续返回stash中的内容，然后发现又其冲突了，看来stash中的东西并不会被修改
        	<<<<<<< Updated upstream
          git stash命令练习2
  
          =======
          git stash命令练习
          >>>>>>> Stashed changes
  ```

+ feature分支

  ```shell
  # 在实际开发中，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
  
  # 额，另外，如果该功能未开发完，项目经理让舍弃掉这个项目。为了保密，以及整个项目代码无杂乱代码，需要把feature分支删除，但是:
  $ git branch -d feature-vulcan
  error: The branch 'feature-vulcan' is not fully merged.
  If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
  # 因为该新项目有内容，且并未与其他分支合并，所以git会提示以上信息
  # 强行删除分支命令:
  $ git branch -D feature-vulcan
  Deleted branch feature-vulcan (was 287773e).
  
  # 完成，今天可以不用加班了
  ```

+ 多人协作：

  ```shell
  # 当从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
  # 要查看远程库的信息，用git remote:
      $git remote
      origin
  # 用git remote -v显示更详细的信息：
      $ git remote -v
      origin  git@github.com:maxwellcatoo/study_text.git (fetch)
      origin  git@github.com:maxwellcatoo/study_text.git (push)
  
  ```

  推送分支：

  ```shell
  $ git push origin master  # 推送master分支
  $ git push origin dev  # 推送dev分支
  # 实际开发中，根据项目要求选择推送需要推送的分支，并不是所有的都需要推送
  master分支是主分支，因此要时刻与远程同步；
  dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
  bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
  feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
  ```

  抓取分支：

  ```shell
  # 情景:在另一台电脑或同一台电脑另一个目录下克隆项目(当然，这个是需要把电脑上的ssh-key配置到远程的GitHub上的):
      $ git clone git@github.com:michaelliao/learngit.git
    Cloning into 'learngit'...
      remote: Counting objects: 40, done.
      remote: Compressing objects: 100% (21/21), done.
      remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
      Receiving objects: 100% (40/40), done.
      Resolving deltas: 100% (14/14), done.
  # 克隆后，默认情况下，只能看到master分支
  	$ git branch
      * master
  # 当我们开发时，要在dev分支上开发，就必须创建远程origin的dev分支到本地，所以用这个命令创建本地dev分支:
  	$ git checkout -b dev origin/dev
  # 之后，就可以在dev分支进行开发，并时不时的把dev分支push到远程:
      $ git add env.txt
  
      $ git commit -m "add env"
      [dev 7a5e5dd] add env
       1 file changed, 1 insertion(+)
       create mode 100644 env.txt
  
      $ git push origin dev
      Counting objects: 3, done.
      Delta compression using up to 4 threads.
      Compressing objects: 100% (2/2), done.
      Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
      Total 3 (delta 0), reused 0 (delta 0)
      To github.com:michaelliao/learngit.git
         f52c633..7a5e5dd  dev -> dev
  # 注意，问题来了，当多人协作时，其中一个人推送了他的dev分支到远程，而另一个人也碰巧对前边那个人提交的文件做了修改，那么推送将会发生错误:
      $ cat env.txt
      env
  
      $ git add env.txt
  
      $ git commit -m "add new env"
      [dev 7bd91f1] add new env
       1 file changed, 1 insertion(+)
       create mode 100644 env.txt
  
      $ git push origin dev
      To github.com:michaelliao/learngit.git
       ! [rejected]        dev -> dev (non-fast-forward)
      error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
      hint: Updates were rejected because the tip of your current branch is behind
      hint: its remote counterpart. Integrate the remote changes (e.g.
      hint: 'git pull ...') before pushing again.
      hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  # 解决办法(就按照上边的提示信息走):
  # 先用git pull把最新的提交从origin/dev抓下来，然后在本地合并，解决冲突，在推送
  	#先git pull时会提示错误:
          $ git pull
          There is no tracking information for the current branch.
          Please specify which branch you want to merge with.
          See git-pull(1) for details.
  
              git pull <remote> <branch>
  
          If you wish to set tracking information for this branch you can do so with:
  
              git branch --set-upstream-to=origin/<branch> dev
      # 原因是没有指定本地dev分支与远程origin/dev分支的链接
      # 所以要先设置dev和origin/dev的链接:
           $ git branch --set-upstream-to=origin/dev dev
           Branch 'dev' set up to track remote branch 'dev' from 'origin'.
      # 然后再pull:
          $ git pull
          Auto-merging env.txt
          CONFLICT (add/add): Merge conflict in env.txt
          Automatic merge failed; fix conflicts and then commit the result.
      # git pull成功，但是合并会有冲突，所以手动解决冲突后，再提交:
          $ git commit -m "fix env conflict"
          [dev 57c53ab] fix env conflict
  
          $ git push origin dev
          Counting objects: 6, done.
          Delta compression using up to 4 threads.
          Compressing objects: 100% (4/4), done.
          Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
          Total 6 (delta 0), reused 0 (delta 0)
          To github.com:michaelliao/learngit.git
             7a5e5dd..57c53ab  dev -> dev
  	# 这样就完成了提交
  ```
  
  一般在多人协作中提交代码时需要做工作：
  
  ```shell
  # 多人协作的工作模式通常是这样:
  
      # 首先，可以试图用git push origin <branch-name>推送自己的修改；
      # 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
      # 如果合并有冲突，则解决冲突，并在本地提交；
      # 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！
      # 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
  ```
  
+ Rebase

  ```shell
  # 多人协作时，提交容易出现冲突，解决冲突后，我们git log查看分支线时，会很乱，有多乱呢，请看:
      $ git log --graph --pretty=oneline --abbrev-commit
      * d1be385 (HEAD -> master, origin/master) init hello
      *   e5e69f1 Merge branch 'dev'
      |\  
      | *   57c53ab (origin/dev, dev) fix env conflict
      | |\  
      | | * 7a5e5dd add env
      | * | 7bd91f1 add new env
      | |/  
      * |   12a631b merged bug fix 101
      |\ \  
      | * | 4c805e2 fix bug 101
      |/ /  
      * |   e1e9c68 merge with no-ff
      |\ \  
      | |/  
      | * f52c633 add merge
      |/  
      *   cf810e4 conflict fixed
  # 把分叉的提交变成直线的方法:
  # 使用git rebase命令
      $ git rebase
      First, rewinding head to replay your work on top of it...
      Applying: add comment
      Using index info to reconstruct a base tree...
      M	hello.py
      Falling back to patching base and 3-way merge...
      Auto-merging hello.py
      Applying: add author
      Using index info to reconstruct a base tree...
      M	hello.py
      Falling back to patching base and 3-way merge...
      Auto-merging hello.py
     
  # 然后，分支线就变成一条直线了，再提交到远程后，远程的东西也会变成一条直线(所以，修改这东西要慎重)
  ```

#### 标签管理

+ 什么是标签：

  ```shell
  # tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
  
  # 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
  
  #Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。
  ```

+ 创建标签：

  ```shell
  # 首先，切换到需要打标签的分支上
  # 使用命令:
    $ git tag 'NAME'     # 注意：该指令默认给最新提交的commit打标签   
      # 即可打一个新标签
  # 然后，可以使用命令:
  	$git tag 	# 查看所有标签
  	v1.0
  # 如果要给之前的提交打标签，则需要添加参数:
  	$ git tag v0.9 f52c633    
  	# f52c633为需要打标签的commit的id
  # 标签不按时间排序，而是按字母排序，所以，标签名一定要足够的有辨识度。要查看某一个标签的具体信息，使用指令:
  	$ git show v1.0
  # 另外，还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字(说明文字同样会在git show指令后显现):
  	$ git tag -a v0.1 -m "version 0.1 released" 1094adb
  	
  	$ git show v0.1
      tag v0.1
      Tagger: Michael Liao <askxuefeng@gmail.com>
      Date:   Fri May 18 22:48:43 2018 +0800
  
      version 0.1 released
  
      commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
      Author: Michael Liao <askxuefeng@gmail.com>
      Date:   Fri May 18 21:06:15 2018 +0800
  
          append GPL
  
      diff --git a/readme.txt b/readme.txt
      ...
  ```
  
+ 操作标签

  ```shell
  # 当标签打错时,可以使用-d删除标签:
  	$git tag -d v1.0
  # 创建的标签只存储在本地，使用git push默认指令不会把标签推送到远程
  # 把标签推送到远程的命令:
      $ git push origin v1.0
      Total 0 (delta 0), reused 0 (delta 0)
      To github.com:michaelliao/learngit.git
       * [new tag]         v1.0 -> v1.0
  # 以及把所有本地未推送到远程标签推送到远程的命令:	
      $ git push origin --tags
      Total 0 (delta 0), reused 0 (delta 0)
      To github.com:michaelliao/learngit.git
       * [new tag]         v0.9 -> v0.9
  # 标签推送到远程后，删除标签的命令:
  	1.# 先删除本地的
          $ git tag -d v0.9
          Deleted tag 'v0.9' (was f52c633)
      2.# 删除远程
      	$ git push origin :refs/tags/v0.9
          To github.com:michaelliao/learngit.git
           - [deleted]         v0.9
  ```

#### 使用GitHub参与开源项目

+ 参与开源项目

  ```shell
  1.# 打开要参与的开源项目的GitHub页，点击右上角的'Fork'，这样就在自己的账号下克隆了该仓库，然后从自己的账号下clone:
  	$ git clone git@github.com:michaelliao/bootstrap.git
  	# 如果不'Fork'，直接clone，是无法提交的。Fork后也要从自己的仓库下进行clone才能提交
  ```

#### 使用Gitee

+ 登录Gitee主页，创建Gitee仓库

  ```shell
  # 全中文页面，自己看着搞就行了，跟GitHub主页很像，地址:
  	https://gitee.com/
  ```

+ 本地项目与Gitee远程库连接

  ```shell
  $ git remote add origin git@gitee.com:liaoxuefeng/learngit.git   # 跟连接GitHub项目操作一样
  
  # 这已经连接好了，就可以git push 、git pull了
  
  # 但，如果本地项目已经与GitHub项目连接，使用上述连接Gitee会报错，需要先取消掉本地与GitHub远程仓库的连接
  	$ git remote -v  # 可以先使用此命令查看远程库信息
      origin	git@github.com:michaelliao/learngit.git (fetch)
      origin	git@github.com:michaelliao/learngit.git (push)
      
      $ git remote rm origin  # 先去除与远程库的连接
      
      $ git remote add origin git@gitee.com:liaoxuefeng/learngit.git  # 再连接Gitee的远程库
  ```

+ 既连接GitHub库，又连接Gitee库

  ```shell
  # 先解除掉与Github的连接
  	$ git remote rm origin
  # 然后，关联Github远程库(注意，不在使用GitHub远程库默认名(要用也可以，不过两者要有区分))
  	$ git remote add github git@github.com:michaelliao/learngit.git
  # 然后，再关联Gitee远程库
  	$ git remote add gitee git@gitee.com:liaoxuefeng/learngit.git
  # 此时，使用git remote -v查看远程库命令:
  	git remote -v
      gitee	git@gitee.com:liaoxuefeng/learngit.git (fetch)
      gitee	git@gitee.com:liaoxuefeng/learngit.git (push)
      github	git@github.com:michaelliao/learngit.git (fetch)
      github	git@github.com:michaelliao/learngit.git (push)
  # 而此时，我们推送本地仓库到远程时，则需要加上远程仓库的名字，不可省略
  	$ git push github master
  	$ git push gitee master
  ```

#### 一些自定义配置

+ 忽略特殊文件

  ```shell
  # 在工作区目录下，创建一个.gitignore文件，然后把要忽略的文件名填进去，那么我们git add和git status查看的时候就会忽略掉这些文件了
  # 举例:
  
      # Windows:
      Thumbs.db
      ehthumbs.db
      Desktop.ini
  
      # Python:
      *.py[cod]
      *.so
      *.egg
      *.egg-info
      dist
      build
  
      # My configurations:
      db.ini
      deploy_key_rsa
  # .gitignore文件中，多使用正则表达式匹配一些文件，有时候可能会忽略掉我们想要提交的文件,解决办法:
  	1.# 使用强制命令
  		$ git add -f App.class   # 加个-f
  	2.# 在配置文件中添加信息
  		!.gitignore
  		!App.class
  		# 这样，被.*和*.class隐藏的.gitignore和App.class文件就可以提交了
  	3.# 可能是配置文件写错的？使用命令检查:
  		$ git check-ignore -v App.class
  		.gitignore:3:*.class	App.class
  		# Git会告诉我们，.gitignore的第3行规则忽略了该文件
  		# 如果确实写错了，那就修改，如果没写错，那就用第二种或第一种
  ```

+ 配置别名(git用的不是很熟练的话不建议用)

  ```shell
  # 令git st 表示git status
  $ git config --global alias.st status
  
  # 现在玩的还不熟，不建议用，等到要用的时候，参看网页链接:https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424
  ```

#### 搭建Git服务器

+ 自己搭建Git服务器

  ```shell
  # 搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装
  # 前提: 要有一个sudo权限的用户账号
  # 步骤:
  	1.# 安装git:
  		$ sudo apt-get install git
  	2.# 创建一个git用户，用来运行git服务:
  		$ sudo adduser git
  	3.# 创建证书登录:
  		# 收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
  	4.# 初始化Git仓库:
  		# 先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令:
  			$ sudo git init --bare sample.git
  			# 此时，git就已经创建了一个裸仓库，裸仓库没有工作区，因为服务器上的git主要是为了共享，不是让使用者直接在工作区修改的
  		# 然后，把owner改为git:
  			$ sudo chown -R git:git sample.git
  	5.# 禁止shell登录:
  		# 出于安全考虑，第二步创建的git用户不允许登录shell，因此通过编辑/etc/passwd文件来完成:
  			# 将文件中的:
  				git:x:1001:1001:,,,:/home/git:/bin/bash
  			# 改为:
  				git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
  		# 这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。
  	6.# 克隆远程仓库
  		# 此时，就已经可以使用git clone命令克隆部署在服务器上的远程仓库的内容了:
              $ git clone git@server:/srv/sample.git
               Cloning into 'sample'...
               warning: You appear to have cloned an empty repository.
  ```

+ 管理公匙
  ```shell
  # 小团队的话，把公匙收集起来放到服务器中的/home/git/.ssh/authorized_keys文件中，如果人很多，比如几百人，那就需要用Gitosis来管理公匙，这里不详细记录(因为别人也没说...)
  ```
  
+ 管理权限
  ```shell
  # 仓库可以通过Gitolite工具达到控制每个人对每个分支甚至每个目录是否有读写权限，这里不详细介绍(因为别人也没说...)
  ```

#### git的图像管理工具

+ #### SourceTree

  ```shell
  # 附上链接:https://www.liaoxuefeng.com/wiki/896043488029600/1317161920364578
  # 我一般直接用安装git时附带的那个Gui工具
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

  

