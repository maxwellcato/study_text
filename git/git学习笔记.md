#### 使用git push到远程仓库，发生如下报错情况

+ ```shell
  To github.com:maxwellcatoo/hospitalManageSystem.git
   ! [rejected]        master -> master (fetch first)
  error: failed to push some refs to 'git@github.com:maxwellcatoo/hospitalManageSystem.git'
  hint: Updates were rejected because the remote contains work that you do
  hint: not have locally. This is usually caused by another repository pushing
  hint: to the same ref. You may want to first integrate the remote changes
  hint: (e.g., 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  
  ```

+ 结局方案：https://blog.csdn.net/wenwen360360/article/details/75566806

+ 发生原因：据说是因为远程仓库的文件比本地的要新，所以没有办法覆盖。这让我很郁闷，因为我已经把远程代码删干净了还是不行，在网上查报错原因，按照大部分网上给的那种答案：1.pull远程仓库代码；2.rebase两个起冲突的版本依然不能解决问题，使用--rebase还导致代码版本链上 的一个版本发生了冲突。后来将本地git文件删除重新git init再连接push 还是不可以。最终按照上面给的答案解决了问题。但是对于为什么发生这种情况，不得而知。（发生此事的原因可能是当时连接仓库时，复制的仓库名称在创建连接时粘贴出现了错误，当时一粘贴就直接运行了仓库连接代码，后来又仓库名，clone仓库代码，pull仓库代码的，导致当时根目录出现了一个仓库名字的文件，里边有.github文件，当时没注意，直接提交了，或许这是发生这种情况的原因。        另外，在我按照上面的方法解决掉，把代码提交到远程仓库时，发现刚提交的代码显示的提交时间是一小时之前，这种异常的情况或许跟以上出现的情况是同一原因导致的。）

#### 在同一台电脑上使用两个git账号

教程出处：

```
作者：一肩月光
链接：https://www.jianshu.com/p/3fc93c16ad2d
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

```
网页我有保存，但是因为之前从来没有接触过这个东西，而且所学习的git书和网络教程中都没有提及这方面的知识，担心到时候这个网站挂掉了，很难找参考资料，所以基本上把这篇文章复制到这里来了。
```



```shell
# 思路
ssh 方式链接到 Github，需要唯一的公钥，如果想同一台电脑绑定两个Github 帐号，需要两个条件:

能够生成两对 私钥/公钥
push 时，可以区分两个账户，推送到相应的仓库
解决方案:

生成 私钥/公钥 时，密钥文件命名避免重复
设置不同 Host 对应同一 HostName 但密钥不同
取消 git 全局用户名/邮箱设置，为每个仓库独立设置 用户名/邮箱
# 操作
1.查看已有 密钥
	Mac 下输入命令 ls ~/.ssh/，看到 id_rsa 与 id_rsa_pub 则说明已经有一对密钥。
	
2.生成新的公钥，并命名为 id_rsa_2 (保证与之前密钥文件名称不同即可)
	ssh-keygen -t rsa -f ~/.ssh/id_rsa_2 -C "yourmail@xxx.com"
	
3.在 .ssh 文件夹下新建 config 文件并编辑，另不同 Host 实际映射到同一 HostName，但密钥文件不同。Host 前缀可自定	义，例子中ieit:
        # default                                                                       
        Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa
        # two                                                                           
        Host ieit.github.com   # ieit名称可自行设置，后边关联仓库时保持同步即可
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa_2
        
4.将生成的 id_rsa.pub，id_rsa_2.pub内容copy 到对应的GitHub账号sshkey中
	
5.测试 ssh 链接
        ssh -T git@ieit.github.com
        ssh -T git@github.com
        # Hi IEIT! You've successfully authenticated, but GitHub does not provide shell access.
        # 出现上边这句，表示链接成功
        
6.将项目 clone 到本地， folder-name 是本地文件夹路径
        git clone git@github.com:whatever folder-name
7.取消全局 用户名/邮箱设置，并进入项目文件夹单独设置
        # 取消全局 用户名/邮箱 配置
        git config –global –unset user.name
        git config –global –unset user.email
        # 单独设置每个repo 用户名/邮箱
        git config user.email “xxxx@xx.com”
        git config user.name “xxxx”
        
        # 这样子的话，以后每个项目都要重新设置一次了，不过也没办法
8.命令行进入项目目录，重建 origin (whatever 为相应项目地址)
        git remote rm origin
        git remote add origin git@ieit.github.com:whatever  # 注意，这里的whatever需要参考你的实际项目内容来写，一般来说，在GitHub仓库中复制地址下来，然后在@后面添上ieit(跟你上边config文件中的配置保持同步).就可以了

9.然后就可以成功提交了。
```



