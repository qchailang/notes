版本库(Repository)又名仓库，可以简单理解成一个目录，里面存放着各种文件，可以被Git进行管理，每个文件的修改、删除，Git都能跟踪。

创建一个版本库::

 $ mkdir repository
 $ cd repository
 $ git init

配置仓库::

 $ git config --global user.name "Your Name"
 $ git config --global user.email email@example.com

简单来说就是先创建目录，然后切到目录下，再把它变成Git可以管理的仓库。此时在这个目录下多了一个.git目录, 在文件夹下是看不到这个目录的，因为它默认是隐藏的，但你可以通过命令行进行访问。

显示中文文件名::

$ git config --global core.quotepath false

git log 不能显示中文::

$ git --no-pager log 如果能显示中文，就使用如下命令：
$ git config --global core.pager more

添加多个远程仓库

第一种方式：

添加一个远程库 名字不能是origin

git remote add xxxx https://qchailang:abc@gitee.com/qchailang/LearningNotes.git
查看远程库及地址
git remote -v 

拉，推

git pull xxxx    远程分支名：本地分支名

git push xxxx   本地分支名：远程分支名


第二种方式：（好处是推送时，可以同时推送到另外一个库）

git remote set-url --add origin https://qchailang:abc@gitee.com/qchailang/LearningNotes.git

推送

git remote -v

git push origin master:master


3.取消本地目录下关联的远程库：

git remote remove origin

一般我们开发项目的时候，从远端clone下来，会默认有一个origin 远程库，输入命令

git remote

这里写图片描述

这个远程仓库我们平时可以提交代码。

现在呢，我们这边分内网和外网，部署在阿里云的代码也需要推送到内网的远程库，所以现在的需求如下：

平时提交代码的同时，还要提交一份到自动化部署的远程库，由于分支命名限制，本地还要先合并代码到一个本地的分支（dev）然后由本分支推送到远程库，下面开始我们的表演。

git remote add originName address

执行成功之后再执行

git remote就可以看到两个远程仓库了，名字是可以随便起的，确保自己可以区分就好。

这里写图片描述

地址可以是ssh，也可以是https，保证自己有对应的权限就好了。

现在你就拥有了向另一个远端推送代码的权利。

下面说一下分支，本地有的分支才可以推送到远端，比如你本地有master和prod分支，你可以直接把这两个分支推送过去。

git push originName master

如果远端的分支你没有呢？那么你本地就要新建一个分支

git checkout -b dev

这时候你就切出来了一个dev分支，这个dev分支的内容和你切换分支之前的内容是一致的。

这时候就可以

git push originName dev

这时，新的远端就有了这一分支和内容。以后再推送的时候就可以直接运行这一命令，而且，你向之前分支的推送也不要直接用

git push了，因为这个指令会向两个分支同时推送。

所以要使用

git push origin dev声明远程仓库。

如果你本地开发的分支不是dev，那样就可以先切换到dev，然后把本地的代码和你开发的分支merge一下就可以提交了。

一定要养成在项目开始就创建.gitignore文件的习惯，否则一旦push，处理起来会非常麻烦。

当然如果已经push了怎么办?当然也有解决方法，如下：

有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交::

 git rm -r --cached .
 git add .
 git commit -m 'xxxxx'

文件.gitignore的格式规范：

* #为注释
* 可以使用shell所使用的正则表达式来进行模式匹配
* 匹配模式最后跟"/"说明要忽略的是目录
* 使用！取反

例子::

  # 此为注释 – 将被 Git 忽略

  *.a       # 忽略所有 .a 结尾的文件
  !lib.a    # 但 lib.a 除外
  /TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
  build/    # 忽略 build/ 目录下的所有文件
  doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt


   Git常用命令清单
配置

首先是配置帐号信息
ssh -T git@github.com #登陆github
修改项目中的个人信息

$ git config --global user.name "wirelessqa"
$ git config --global user.email wirelessqa.me@gmail.com
config

git config --global user.name JSLite #设置提交用户名
git config --global user.email JSLite@yeah.net #设置提交邮箱
git config --list #查看配置的信息
git remote remove origin #删除该远程路径
git remote add origin git@jslite.github.com:JSLite/JSLite.git #添加远程路径
help

git help config #获取帮助信息
配置自动换行（自动转换坑太大）

git config --global core.autocrlf input #提交到git是自动将换行符转换为lf
配置密钥

ssh-keygen -t rsa -C JSLite@yeah.net #生成密钥
ssh -T git@github.com #测试是否成功
多账号ssh配置
1.生成指定名字的密钥

ssh-keygen -t rsa -C "邮箱地址" -f ~/.ssh/github_jslite
会生成 github_jslite 和 github_jslite.pub 这两个文件
2.密钥复制到托管平台上

vim ~/.ssh/github_jslite.pub
打开公钥文件 github_jslite.pub ，并把内容复制至代码托管平台上
3.修改config文件

vim ~/.ssh/config #修改config文件，如果没有创建 config

Host jslite.github.com
HostName github.com
User git
IdentityFile ~/.ssh/github_jslite

Host abc.github.com
HostName github.com
User git
IdentityFile ~/.ssh/github_abc

4.测试

ssh -T git@jslite.github.com # @后面跟上定义的Host
Git推向3个库
增加3个远程库地址

git remote add origin https://github.com/JSLite/JSLite.git
git remote set-url --add origin https://gitlab.com/wang/JSLite.js.git
git remote set-url --add origin https://oschina.net/wang/JSLite.js.git
删除其中一个 set-url 地址

usage: git remote set-url [--push] <name> <newurl> [<oldurl>]
   or: git remote set-url --add <name> <newurl>
   or: git remote set-url --delete <name> <url>

git remote set-url --delete origin https://oschina.net/wang/JSLite.js.git
push

git push origin master
git push -f origin master #强制推送

    缩写 -f
    全写--force
    注：强制推送文件没有了哦

pull

只能拉取 origin 里的一个url地址，这个fetch-url
默认为你添加的到 origin的第一个地址

git pull origin master
git pull --all #获取远程所有内容包括tag
git pull origin next:master #取回origin主机的next分支，与本地的master分支合并
git pull origin next #远程分支是与当前分支合并

上面一条命令等同于下面两条命令
git fetch origin
git merge origin/next

如果远程主机删除了某个分支，默认情况下，git pull 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致git pull不知不觉删除了本地分支。
但是，你可以改变这个行为，加上参数 -p 就会在本地删除远程已经删除的分支。

$ git pull -p
# 等同于下面的命令
$ git fetch --prune origin 
$ git fetch -p

更改pull

只需要更改config文件里，那三个url的顺序即可，fetch-url会直接对应排行第一的那个utl连接。
新建仓库
init

git init #初始化
status

git status #获取状态
add

git add file #.或*代表全部添加
git rm --cached <added_file_to_undo> 在commit之前撤销git add操作
git reset head 好像比上面git rm --cached更方便
commit

git commit -m "message" #此处注意乱码
remote

git remote add origin git@github.com:JSLite/test.git #添加源
push

git push -u origin master #push同事设置默认跟踪分支
git push origin master
从现有仓库克隆

git clone git://github.com/JSLite/JSLite.js.git
git clone git://github.com/JSLite/JSLite.js.git mypro #克隆到自定义文件夹
git clone [user@]example.com:path/to/repo.git/ #SSH协议还有另一种写法。

git clone支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等，下面是一些例子。$ git clone <版本库的网址> <本地目录名>

$ git clone http[s]://example.com/path/to/repo.git/
$ git clone ssh://example.com/path/to/repo.git/
$ git clone git://example.com/path/to/repo.git/
$ git clone /opt/git/project.git 
$ git clone file:///opt/git/project.git
$ git clone ftp[s]://example.com/path/to/repo.git/
$ git clone rsync://example.com/path/to/repo.git/

submodule

git submodule add --force 仓库地址 路径
其中，仓库地址是指子模块仓库地址，路径指将子模块放置在当前工程下的路径。
注意：路径不能以 / 结尾（会造成修改不生效）、不能是现有工程已有的目录（不能順利 Clone）
git submodule init 初始化submodule
git submodule update 更新submodule(必须在根目录执行命令)

当使用git clone下来的工程中带有submodule时，初始的时候，submodule的内容并不会自动下载下来的，此时，只需执行如下命令：
git submodule update --init --recursive 下载的工程带有submodule

git submodule foreach git pull submodule 里有其他的 submodule 一次更新
git submodule foreach git pull origin master submodule更新

git submodule foreach --recursive git submodule init
git submodule foreach --recursive git submodule update
本地
add

git add * #跟踪新文件
git add -u [path] #添加[指定路径下]已跟踪文件
rm

rm *&git rm * #移除文件
git rm -f * #移除文件
git rm --cached * #取消跟踪
git mv file_from file_to #重命名跟踪文件
git log #查看提交记录
commit

git commit #提交更新
git commit -m 'message' #提交说明
git commit -a #跳过使用暂存区域，把所有已经跟踪过的文件暂存起来一并提交
git commit --amend #修改最后一次提交
git commit log #查看所有提交，包括没有push的commit
git commit -m "#133" #关联issue 任意位置带上# 符号加上issue号码
git commit -m "fix #133" commit关闭issue
git commit -m '概要描述'$'\n\n''1.详细描述'$'\n''2.详细描述' #提交简要描述和详细描述
reset

git reset HEAD *#取消已经暂存的文件
git reset --mixed HEAD *#同上
git reset --soft HEAD *#重置到指定状态，不会修改索引区和工作树
git reset --hard HEAD *#重置到指定状态，会修改索引区和工作树
git reset -- files *#重置index区文件
那么如何跟随着commit关闭一个issue呢? 在confirm merge的时候可以使用一下命令来关闭相关issue:
1. fixes #xxx 1. fixed #xxx 1. fix #xxx 1. closes #xxx 1. close #xxx 1. closed #xxx
revert

git revert HEAD #撤销前一次操作
git revert HEAD~ #撤销前前一次操作
git revert commit ##撤销指定操作
checkout

git checkout -- file #取消对文件的修改（从暂存区——覆盖worktree file）
git checkout branch|tag|commit -- file_name #从仓库取出file覆盖当前分支
git checkout HEAD~1 [文件] #将会更新 working directory 去匹配某次 commit
git checkout -- . #从暂存区取出文件覆盖工作区
git checkout -b gh-pages 0c304c9 这个表示 从当前分支 commit 哈希值为 0c304c9 的节点，分一个新的分支gh-pages出来，并切换到 gh-pages
diff

git diff file #查看指定文件的差异
git diff --stat #查看简单的diff结果
git diff #比较Worktree和Index之间的差异
git diff --cached #比较Index和HEAD之间的差异
git diff HEAD #比较Worktree和HEAD之间的差异
git diff branch #比较Worktree和branch之间的差异
git diff branch1 branch2 #比较两次分支之间的差异
git diff commit commit #比较两次提交之间的差异
$ git diff master..test #上面这条命令只显示两个分支间的差异
git diff master...test #你想找出‘master’,‘test’的共有 父分支和'test'分支之间的差异，你用3个‘.'来取代前面的两个'.'
stash

git stash #将工作区现场（已跟踪文件）储藏起来，等以后恢复后继续工作。
git stash list #查看保存的工作现场
git stash apply #恢复工作现场
git stash drop #删除stash内容
git stash pop #恢复的同时直接删除stash内容
git stash apply stash@{0} #恢复指定的工作现场，当你保存了不只一份工作现场时。
merge

git merge --squash test ##合并压缩，将test上的commit压缩为一条
cherry-pick

git cherry-pick commit #拣选合并，将commit合并到当前分支
git cherry-pick -n commit #拣选多个提交，合并完后可以继续拣选下一个提交
rebase

git rebase master #将master分之上超前的提交，变基到当前分支
git rebase --onto master 169a6 #限制回滚范围，rebase当前分支从169a6以后的提交
git rebase --interactive #交互模式，修改commit
git rebase --continue #处理完冲突继续合并
git rebase --skip #跳过
git rebase --abort #取消合并
分支branch
删除

git push origin :branchName #删除远程分支
git push origin --delete new #删除远程分支new
git branch -d branchName #删除本地分支，强制删除用-D
git branch -d test #删除本地test分支
git branch -D test #强制删除本地test分支
提交

git push -u origin branchName #提交分支到远程origin主机中
拉取

git fetch -p #拉取远程分支时，自动清理 远程分支已删除，本地还存在的对应同名分支。
分支合并

git merge branchName #合并分支 - 将分支branchName和当前所在分支合并
git merge origin/master #在本地分支上合并远程分支。
git rebase origin/master #在本地分支上合并远程分支。
git merge test #将test分支合并到当前分支
重命名

git branch -m old new #重命名分支
查看

git branch #列出本地分支
git branch -r #列出远端分支
git branch -a #列出所有分支
git branch -v #查看各个分支最后一个提交对象的信息
git branch --merge #查看已经合并到当前分支的分支
git branch --no-merge #查看为合并到当前分支的分支
新建

git branch test #新建test分支
git checkout -b newBrach origin/master #取回远程主机的更新以后，在它的基础上创建一个新的分支
连接

git branch --set-upstream dev origin/dev #将本地dev分支与远程dev分支之间建立链接
git branch --set-upstream master origin/next #手动建立追踪关系
分支切换

git checkout test #切换到test分支
git checkout -b test #新建+切换到test分支
git checkout -b test dev #基于dev新建test分支，并切换
远端

git fetch <远程主机名> <分支名> #fetch取回所有分支（branch）的更新
git fetch origin remotebranch[:localbranch] # 从远端拉去分支[到本地指定分支]
git merge origin/branch #合并远端上指定分支
git pull origin remotebranch:localbranch # 拉去远端分支到本地分支
git push origin branch #将当前分支，推送到远端上指定分支
git push origin localbranch:remotebranch #推送本地指定分支，到远端上指定分支
git push origin :remotebranch #删除远端指定分支
git checkout -b [--track] test origin/dev 基于远端dev分支，新建本地test分支[同时设置跟踪]
撤销远程记录

git reset --hard HEAD~1 #撤销一条记录
git push -f origin HEAD:master #同步到远程仓库
忽略文件

echo node_modules/ >> .gitignore
删除文件

git rm -rf node_modules/
源remote

git是一个分布式代码管理工具，所以可以支持多个仓库，在git里，服务器上的仓库在本地称之为remote。
个人开发时，多源用的可能不多，但多源其实非常有用。
git remote add origin1 git@github.com:yanhaijing/data.js.git
git remote #显示全部源
git remote -v #显示全部源+详细信息
git remote rename origin1 origin2 #重命名
git remote rm origin #删除
git remote show origin #查看指定源的全部信息
同步一个fork

github教程
在github上同步一个分支(fork)
设置

在同步之前，需要创建一个远程点指向上游仓库(repo).如果你已经派生了一个原始仓库，可以按照如下方法做。

$ git remote -v
# List the current remotes （列出当前远程仓库）
# origin  https://github.com/user/repo.git (fetch)
# origin  https://github.com/user/repo.git (push)
$ git remote add upstream https://github.com/otheruser/repo.git
# Set a new remote (设置一个新的远程仓库)
$ git remote -v
# Verify new remote (验证新的原唱仓库)
# origin    https://github.com/user/repo.git (fetch)
# origin    https://github.com/user/repo.git (push)
# upstream  https://github.com/otheruser/repo.git (fetch)
# upstream  https://github.com/otheruser/repo.git (push)

同步

同步上游仓库到你的仓库需要执行两步：首先你需要从远程拉去，之后你需要合并你希望的分支到你的本地副本分支。

从上游的存储库中提取分支以及各自的提交内容。 master 将被存储在本地分支机构 upstream/master

git fetch upstream
# remote: Counting objects: 75, done.
# remote: Compressing objects: 100% (53/53), done.
# remote: Total 62 (delta 27), reused 44 (delta 9)
# Unpacking objects: 100% (62/62), done.
# From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
#  * [new branch]      master     -> upstream/master

检查你的 fork's 本地 master 分支

git checkout master
# Switched to branch 'master'

合并来自 upstream/master 的更改到本地 master 分支上。 这使你的前 fork's master 分支与上游资源库同步，而不会丢失你本地修改。

git merge upstream/master
# Updating a422352..5fdff0f
# Fast-forward
#  README                    |    9 -------
#  README.md                 |    7 ++++++
#  2 files changed, 7 insertions(+), 9 deletions(-)
#  delete mode 100644 README
#  create mode 100644 README.md

标签tag

当开发到一定阶段时，给程序打标签是非常棒的功能。
git tag #列出现有标签
git tag v0gi.1 #新建标签
git tag -a v0.1 -m 'my version 1.4' #新建带注释标签
git checkout tagname #切换到标签
git push origin v1.5 #推送分支到源上
git push origin --tags #一次性推送所有分支
git tag -d v0.1 #删除标签
git push origin :refs/tags/v0.1 #删除远程标签
git pull --all #获取远程所有内容包括tag
git --git-dir='<绝对地址>/.git' describe --tags HEAD #查看本地版本信息
日志log

git config format.pretty oneline #显示历史记录时，每个提交的信息只显示一行
git config color.ui true #彩色的 git 输出
git log #查看最近的提交日志
git log --pretty=oneline #单行显示提交日志
git log --graph --pretty=oneline --abbrev-commit
git log -num #显示第几条log（倒数）
git reflog #查看所有分支的所有操作记录
git log --since=1.day #一天内的提交；你可以给出各种时间格式，比如说具体的某一天（“2008-01-15”），或者是多久以前（“2 years 1 day 3 minutes ago”）。
git log --pretty="%h - %s" --author=自己的名字 #查看自己的日志
git log -p -2 #展开两次更新显示每次提交的内容差异
git log --stat #要快速浏览其他协作者提交的更新都作了哪些改动
git log --pretty=format:"%h - %an, %ar : %s"#定制要显示的记录格式
git log --pretty=format:'%h : %s' --date-order --graph#拓扑顺序展示
git log --pretty=format:'%h : %s - %ad' --date=short #日期YYYY-MM-DD显示
git log <last tag> HEAD --pretty=format:%s 只显示commit
选项	说明
%H 	提交对象（commit）的完整哈希字串
%h 	提交对象的简短哈希字串
%T 	树对象（tree）的完整哈希字串
%t 	树对象的简短哈希字串
%P 	父对象（parent）的完整哈希字串
%p 	父对象的简短哈希字串
%an 	作者（author）的名字
%ae 	作者的电子邮件地址
%ad 	作者修订日期（可以用 -date= 选项定制格式）
%ar 	作者修订日期，按多久以前的方式显示
%cn 	提交者(committer)的名字
%ce 	提交者的电子邮件地址
%cd 	提交日期
%cr 	提交日期，按多久以前的方式显示
%s 	提交说明
重写历史

git commit --amend #改变最近一次提交
git rebase -i HEAD~3 #修改最近三次的提交说明，或者其中任意一次
git commit --amend #保存好了，这些指示很明确地告诉了你该干什么
git rebase --continue 修改提交说明，退出编辑器。

pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

改成

pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit

查看某个文件历史

git log --pretty=oneline 文件名 #列出文件的所有改动历史
git show c178bf49 #某次的改动的修改记录
git log -p c178bf49 #某次的改动的修改记录
git blame 文件名 #显示文件的每一行是在那个版本最后修改。
git whatchanged 文件名 #显示某个文件的每个版本提交信息：提交日期，提交人员，版本号，提交备注（没有修改细节）
打造自己的git命令

git config --global alias.st status
git config --global alias.br branch
git config --global alias.co checkout
git config --global alias.ci commit

配置好后再输入git命令的时候就不用再输入一大段了，例如我们要查看状态，只需：

git st

总结

git help * #获取命令的帮助信息
git status #获取当前的状态，非常有用，因为git会提示接下来的能做的操作
报错

    git fatal: protocol error: bad line length character: No s 解决办法：更换remote地址为 http/https 的
    The requested URL returned error: 403 Forbidden while accessing解决github push错误的办法

解决方案：

#vim 编辑器打开 当前项目中的config文件
vim .git/config

#修改
[remote "origin"]  
    url = https://github.com/jaywcjlove/example.git  

#为下面代码
[remote "origin"]  
    url = https://jaywcjlove@github.com/jaywcjlove/example.git  

git commit -m “注释” 提交的文件 带m不用进入到vim中

git log 查看历史记录 space(向下翻页) b(向上翻页)p(向下翻页)

git log --pretty=online 显示一行全部

git log --online 显示一行但是哈希值显示一部分

git reflog 比git log --online多显示了指针,步数

git reset --hard 部分哈希值

git reset --hard[索引值] 可以前进可以后退

git reset --hard HEAD^ 只能后退,有几个^就后退几步

git reset --hard HEAD~n 表示后退n步

git help reset查看本地文档

reset 三个参数

 --soft 仅仅在本库中移动指针

 --mixed 在本库中移动指针 重置缓冲区

 --hard 在 本库中移动指针 重置缓冲区 重置工作区

git diff[文件名] 将工作区文件和暂存区进行比较

git diff[本地库中历史版本] [文件名] 将工作去文件和本地库历史版本进行比较

不带文件名可以与多个文件进行比较

分支

 git branch -v 查看分支

 git branch 分支名 创建分支

 git checkout 分支名 切换分支

 git branch -d 分支名 删除分支

 git branch -a 查看所有分支名

git remote -v 查看当前所有远程地址别名

git remote add [别名] [远程地址]

 git remote add origin git下载地址

git push [别名] [远程地址]

git config --system --unset credential.helper 弹出登录窗口

git pull --rebase origin master 从远程仓库来下来并与本地仓库合并

ssh -keygen -t rsa -C “邮箱地址” 生成密钥

ssh -T git@gitee.com 测试码云是否成功

git push -u origin master 推送并提交

5.设置签名
作用是区分不同人员的开发身份

分为1.项目级别(仓库级别) 2.系统级别

设置的用户签名信息被保存在git/config文件中

6.哈希的特点
哈希是一个系列的加密算法,各个不同的哈希算法虽然加密程度强度不同,但是有一下几个共同点

1.不管输入数据的数据量有多大,输入同一个哈希算法,得到的加密结果长度固定(一系列的数学运算在里面)

2.哈希算法固定,输入数据固定,输出数据能够保证不变

3.哈希算法确定,输入数据有变化,输出数据一定有变化,而且通常变化很大

4.哈希算法不可逆

Git底层采用的是SHA-1算法.

哈希算法可以被用来验证文件
