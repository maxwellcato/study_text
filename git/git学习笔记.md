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