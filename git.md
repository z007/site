```
参考 git ssh key
```    
```

使用git clone命令从github上同步github上的代码库时，如果使用SSH链接（如我自己的beagleOS项目：git@github.com:DamonDeng/beagleOS.git），而你的SSH key没有添加到github帐号设置中，系统会报下面的错误：
Permission denied (publickey).
fatal: The remote end hung up unexpectedly

这时需要在本地创建SSH key，然后将生成的SSH key文件内容添加到github帐号上去。
创建SSH key的方法很简单，执行如下命令就可以：
ssh-keygen
然后系统提示输入文件保存位置等信息，连续敲三次回车即可，生成的SSH key文件保存在中～/.ssh/id_rsa.pub

然后用文本编辑工具打开该文件，我用的是vim,所以命令是：
vim ~/.ssh/id_rsa.pub

接着拷贝.ssh/id_rsa.pub文件内的所以内容，将它粘帖到github帐号管理中的添加SSH key界面中。
打开github帐号管理中的添加SSH key界面的步骤如下：
1. 登录github
2. 点击右上方的Accounting settings图标
3. 选择 SSH key
4. 点击 Add SSH key
在出现的界面中填写SSH key的名称，填一个你自己喜欢的名称即可，然后将上面拷贝的~/.ssh/id_rsa.pub文件内容粘帖到key一栏，在点击“add key”按钮就可以了。
添加过程github会提示你输入一次你的github密码

添加完成后再次执行git clone就可以成功克隆github上的代码库了。
```
```
第一次运行前要做的系统设置
使用Git 前，要做一些一次性设置。这些设置对整个系统都有效，因此一台电脑只需设置一次：

$ git config --global user.name "Your Name"
$ git config --global user.email your.email@example.com
```
```
第一次使用仓库前要做的设置
下面的步骤每次新建仓库时都要执行。首先进入第一个应用的根目录，然后初始化一个新仓库：

$ git init
Initialized empty Git repository in /home/ubuntu/workspace/hello_app/.git/
然后执行 git add -A 命令，把项目中的所有文件都放到仓库中：

$ git add -A
这个命令会把当前目录中的所有文件都放到仓库中，但是匹配特殊文件 .gitignore 中模式的文件除外。rails new 命令会自动生成一个适用于 Rails 项目的 .gitignore 文件，而且你还可以添加其他模式。[10]

加入仓库的文件一开始位于“暂存区”（staging area），这一区用于存放待提交的内容。执行 status 命令可以查看暂存区中有哪些文件：

$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   .gitignore
  new file:   Gemfile
  new file:   Gemfile.lock
  new file:   README.rdoc
  new file:   Rakefile
  .
  .
  .
（显示的内容很多，所以我使用竖排点号省略了一些内容。）

如果想告诉 Git 保留这些改动，可以使用 commit 命令：

$ git commit -m "Initialize repository"
[master (root-commit) df0a62f] Initialize repository
.
.
.
旗标 -m 的意思是为这次提交添加一个说明。如果没指定 -m 旗标，Git 会打开系统默认使用的编辑器，让你在其中输入说明。（本书所有的示例都会使用 -m 旗标。）

有一点很重要要注意：Git 提交只发生在本地，也就是说只在执行提交操作的设备中存储内容。1.4.4 节会介绍如何把改动推送（使用 git push 命令）到远程仓库中。

顺便说一下，可以使用 log 命令查看提交的历史：

$ git log
commit df0a62f3f091e53ffa799309b3e32c27b0b38eb4
Author: Michael Hartl <michael@michaelhartl.com>
Date:   Wed August 20 19:44:43 2014 +0000

    Initialize repository
如果仓库的提交历史很多，可能需要输入 q 退出。

1.4.2. 使用 Git 有什么好处
如果以前从未用过版本控制，现在可能不完全明白版本控制的好处。那我举个例子说明一下吧。假如你不小心做了某个操作，例如把重要的 app/controllers/ 文件夹删除了：

$ ls app/controllers/
application_controller.rb  concerns/
$ rm -rf app/controllers/
$ ls app/controllers/
ls: app/controllers/: No such file or directory
我们用 Unix 中的 ls 命令列出 app/controllers/ 文件夹里的内容，然后用 rm 命令删除这个文件夹。旗标 -rf 的意思是“强制递归”，无需明确征求同意就递归删除所有文件、文件夹和子文件夹等。

查看一下状态，看看发生了什么：

$ git status
On branch master
Changed but not updated:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

      deleted:    app/controllers/application_controller.rb

no changes added to commit (use "git add" and/or "git commit -a")
可以看出，删除了一个文件。但是这个改动只发生在“工作树”中，还未提交到仓库。所以，我们可以使用 checkout 命令，并指定 -f 旗标，强制撤销这次改动：

$ git checkout -f
$ git status
# On branch master
nothing to commit (working directory clean)
$ ls app/controllers/
application_controller.rb  concerns/
删除的文件夹和文件又回来了，这下放心了！

1.4.3. Bitbucket
我们已经把项目纳入 Git 版本控制系统了，接下来可以把代码推送到 Bitbucket 中。Bitbucket 是一个专门用来托管和分享 Git 仓库的网站。（本书前几版使用 GitHub，换用 Bitbucket 的原因参见旁注 1.4。）在 Bitbucket 中放一份 Git 仓库的副本有两个目的：其一，对代码做个完整备份（包括所有提交历史）；其二，便于以后协作。

旁注 1.4：GitHub 和 Bitbucket

目前，托管 Git 仓库最受欢迎的网站是 GitHub 和 Bitbucket。这两个网站有很多相似之处：都可托管仓库，也可以协作，而且浏览和搜索仓库很方便。但二者之间有个重要的区别（对本书而言）：GitHub 为开源项目提供无限量的免费仓库，但私有仓库收费；而 Bitbucket 提供了无限量的私有仓库，仅当协作者超过一定数量时才收费。所以，选择哪个网站，取决于具体的需求。

本书前几版使用 GitHub，因为它对开源项目来说有很多好用的功能，但我越来越关注安全，所以推荐所有 Web 应用都放在私有仓库中。因为 Web 应用的仓库中可能包含潜在的敏感信息，例如密钥和密码，可能会威胁到使用这份代码的网站的安全。当然，这类信息也有安全的处理方法，但是容易出错，而且需要很多专业知识。

本书开发的演示应用可以安全地公开，但这只是特例，不能推广。因此，为了尽量提高安全，我们不能冒险，还是默认就使用私有仓库保险。既然 GitHub 对私有仓库收费，而 Bitbucket 提供了不限量的免费私有仓库，就我们的需求来说，Bitbucket 比 Github 更合适。

Bitbucket 的使用方法很简单：

如果没有账户，先注册一个 Bitbucket 账户；

把公钥复制到剪切板。云端 IDE 用户可以使用 cat 命令查看公钥，如代码清单 1.11 所示，然后选中公钥，复制。如果你在自己的系统中，执行代码清单 1.11 中的命令后没有输出，请参照“如何在你的 Bitbucket 账户中设定公钥”；

点击右上角的头像，选择“Manage account”（管理账户），然后点击“SSH keys”（SSH 密钥），如图 1.13 所示。

add public key
图 1.13：添加 SSH 公钥
代码清单 1.11：使用 cat 命令打印公钥
$ cat ~/.ssh/id_rsa.pub
添加公钥之后，点击“Create”（创建）按钮，新建一个仓库，如图 1.14 所示。填写项目的信息时，记得要选中“This is a private repository”（这是私有仓库）。填完后点击“Create repository”（创建仓库）按钮，然后按照“Command line > I have an existing project”（命令行 > 现有项目）下面的说明操作，如代码清单 1.12 所示。（如果与代码清单 1.12 不同，可能是公钥没正确添加，我建议你再试一次。）推送仓库时，如果询问“Are you sure you want to continue connecting (yes/no)?”（确定继续连接吗？），输入“yes”。

代码清单 1.12：添加 Bitbucket，然后推送仓库
$ git remote add origin git@bitbucket.org:<username>/hello_app.git
$ git push -u origin --all # 第一次推送仓库
这段代码的意思是，先告诉 Git，你想添加 Bitbucket，作为这个仓库的源，然后再把仓库推送到这个远端的源。（别管 -u 旗标的意思，如果好奇，可以搜索“git set upstream”。）当然了，你要把 <username> 换成你自己的用户名。例如，我运行的命令是：

$ git remote add origin git@bitbucket.org:mhartl/hello_app.git
```
### 参考
    
[git1](http://railstutorial-china.org/book/chapter1.html#version-control-with-git)     
[git2](http://railstutorial-china.org/book/chapter1.html#version-control-with-git)      
[git3](http://rogerdudler.github.io/git-guide/index.zh.html)    
[git4](http://gitref.org/zh/basic/#commit)  
[add ssh](http://blog.csdn.net/keyboardota/article/details/7603630)     
[ssh登录github](http://www.tuicool.com/articles/IrIzamU)    
[使用git建立远程仓库，让别人git clone下来 ](http://blog.sina.com.cn/s/blog_6405313801011vsj.html)     
[Github 中使用 SSH clone URL](http://www.kaixuan.me/archives/333)

[git 自动换行](http://blog.csdn.net/igorzhang/article/details/17420949)
[git commmit -m注释中换行问题 就是在引号里面换行即可](http://blog.csdn.net/longxiaowu/article/details/42146061)

[git checkout](http://www.tuicool.com/articles/A3Mn6f)  

[git checkout2](http://www.cnblogs.com/craftor/archive/2012/11/04/2754147.html)
