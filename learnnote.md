# git学习文档
## 一、git --help文档


    usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]


**These are common Git commands used in various situations:**(一些常见的git命令)


start a working area (see also: git help tutorial)
开始一个工作区(新建一个仓库)

    clone      Clone a repository into a new directory
    init       Create an empty Git repository or reinitialize an existing one


work on the current change (see also: git help everyday)
对当前暂存区的更改

    add        Add file contents to the index
    mv         Move or rename a file, a directory, or a symlink
    reset      Reset current HEAD to the specified state
    rm         Remove files from the working tree and from the index


examine the history and state (see also: git help revisions)
检查历史和状态

    bisect     Find by binary search the change that introduced a bug
    grep       Print lines matching a pattern
    log        Show commit logs
    show       Show various types of objects
    status     Show the working tree status


grow, mark and tweak your common history

    branch     List, create, or delete branches
    checkout   Switch branches or restore working tree files
    commit     Record changes to the repository
    diff       Show changes between commits, commit and working tree, etc
    merge      Join two or more development histories together
    rebase     Forward-port local commits to the updated upstream head
    tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
协作协作

    fetch      Download objects and refs from another repository
    pull       Fetch from and integrate with another repository or a local branch
    push       Update remote refs along with associated objects


'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.


## 二、git使用简介
**这是从廖雪峰老师的网站中摘录的，在此致谢**    [廖雪峰网站地址](http://www.liaoxuefeng.com/)

### 安装git
这里只介绍windows下的安装，msysgit是Windows版的Git，从 http://msysgit.github.io 下载，然后按默认选项安装即可。
### 版本库的建立
1. 创建本地版本库
repository就是我们的软件仓库，在本地建立一个仓库非常简单，只要你在任意的文件夹内输入`git init`就会建立一个以该文件夹名字相同的版本库。
    * 注意在这个文件夹内有一个git的文件夹，这个是git用来跟踪及保存这个仓库必要细腻的文件夹，请不要删除
2. 向仓库中添加文件
    * 版本管理系统只能跟踪纯文本文件的变化，而二进制的变化是无法获悉的，只能知道发生变化而无法知道改变了什么
    * 不要使用windows自带的notepad，它打开文件的方式很傻
    * 文件的编码设置为utf-8,而保存方式设置为utf-8 without BOM
3. 提交文件
    1. 向暂存区提交文件
    `git add filename` 命令用来将工作区的文件提交至暂存区
    2. 向仓库提交文件
    `git commit \[-m\] \[本次提交说明\] ` 一次性将暂存区内的文件正式提交至仓库

### 各个版本的切换（时光穿梭）
#### 1. 查看当前的状态
`git status` 命令能够告诉你当前状态是什么
1. 示例1

    On branch master     说明当前的分支
    Initial commit       说明commit点

    Untracked files:     说明有未向暂存区提交的文件
      (use "git add <file>..." to include in what will be committed) 提交语句提示

    nothing added to commit but untracked files present (use "git add" to track)  

2、示例2

    On branch master

    Initial commit

    Changes to be committed:      说明有未向仓库提交的文件
      (use "git rm --cached <file>..." to unstage)   可以通过rm --cached 命令删除暂存区

        new file:   learnnote.md   文件名称

    Changes not staged for commit:  说明文件有修改，但是未向缓存区提交
      (use "git add <file>..." to update what will be committed) 提交命令说明
      (use "git checkout -- <file>..." to discard changes in working directory) 放弃修改名利

        modified:   learnnote.md   文件名称

**这里有几个命令需要留意**

*  `git checkout -- <filename>` 这个命令是取消工作区的修改，修改后与暂存区一致 
*  `git rm --cachedd <filename>` 
    这个命令是删除暂存区的文件，如果暂存区的文件和工作区的文件不同，系统会提示你，如果想要强制删除需要`-f`选项
*  当提示有修改的时候会显示` modified:   learnnote.md ` ,我们通过`git diff`命令来查看修改的内容,git用-,+分别表示删除和增加的内容

```     
    @@ -103,7 +103,7 @@ repository就是我们的软件仓库，在本地建立一个仓库非常简单
     
             new file:   learnnote.md   文件名称
     
    -    Changes not staged for commit:  说明有未向缓存区提交的文件
    +    Changes not staged for commit:  说明文件有修改，但是未向缓存区提交
           (use "git add <file>..." to update what will be committed) 提交命令说明
           (use "git checkout -- <file>..." to discard changes in working directory) 放弃修改名利
     
    @@ -113,3 +113,7 @@ repository就是我们的软件仓库，在本地建立一个仓库非常简单
         1. `git checkout -- <filename>` 这个命令是取消工作区的修改，修改后与暂存区一致 
         2. `git rm --cachedd <filename>` 
         这个命令是删除暂存区的文件，如果暂存区的文件和工作区的文件不同，系统会提示你，如果想要强制删除需要`-f`选项
    +    3. 当提示有修改的时候会显示` modified:   learnnote.md ` ,我们通过`git diff`命令来查看修改的内容
```

#### 2. 版本回退

1. **历史版本查看**
我们每次`commit`都会产生一个快照，因此就为我们回退版本提供了依据，但是我们不会记得每次回退的ID，所以git提供了历史产看的功能
    * `git log` 查看所有历史记录
    * `git log --pretty=oneline` 每次显示一行记录，精简查看

2. **版本回退**
    * git中用HEAD表示当前的版本，用HEAD^表示上一次的版本，用HEAD^^表示上上次版本，用HEAD~100表示上100次版本。注意的是HEAD指的是仓库中的版本与工作区和暂存区没有关系
    * `git reset --hard HEAD^`  该命令将版本回复至上一次提交的版本。** 注意恢复到某个版本时，这个版本之后的版本将会消失 **
    * 如果你后悔了，那么找到相应的commit号即可，如果你关机了，那么请调用`git reflog`查找当时的记录
    * 注意log是用来退回某个过去的版本，reflog是用来回退至某个未来的版本。

#### 3. 工作区与暂存区

1. 工作区(work directory)
git的工作区就是当前仓库的目录(指的是电脑里面的实体目录)，在这里的任何操作都是在工作区上的操作，比如更改文件、删除文件等等。

2. 版本库 (Repository)
git的目录下又一个.git的目录，这个目录不属于工作区，它是git的本地库
Git的版本库里存了很多东西，其中最重要的就是称为**stage**（或者叫index）的**暂存区**，还有Git为我们自动创建的第一个分支**master**，以及指向master的一个指针叫**HEAD**。

3. 我们提交版本的时候的二个步骤

    * `git add file` 是把文件提交至暂存区
    * `git commit` 是把暂存区的所有内容提交至当前分支
    ![工作区、暂存区、分支的关系](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

#### 4. 管理修改

**git与其他版本控制软件的区别是，git管理的是修改而非文件**

1. `git diff HEAD -- filename` 可以比较当前工作区的文件与版本库的区别**注意:`git diff`是比较工作区与暂存区的区别**
2. `git checkout -- file` 可以丢弃工作区的修改,这里有二种情况
    *第一种，文件修改后未提交值暂存区，那么使用该命令就是将文件恢复与版本库一致
    *第二种，文件提交至暂存区后又修改了，那么该命令是将文件恢复至于暂存区一致
    *这个命令的 -- 符号很重要，如果没有这个符号就变成切换分支的操作了
3. 如果我们将文件放入暂存区了，如果要取消那么需要输入`git reset HEAD file`,就会把暂存区的修改取消，`git reset`既可以回退版本也可以取消暂存区。然后再用工作区恢复命令`git checkout -- file`将内容恢复至上次的版本。
    * 注意这里取消暂存区的意思就是将暂存区的内容删除，不会影响工作区(不过这里我有一点不明白为什么用HEAD,HEAD不是特指版本库的么？)
    * 不影响工作区的意思是你可以在add后继续修改文档，如果一旦发现文档错误你可以将暂存区清空，并手工修改文档或用其他命令恢复至其他版本。

#### 5. 删除文件

在git中删除文件也是一个修改操作，例如你增加了一个test.txt文件至版本库后，在工作区删除了它，这时git会发现你的版本不同了

    On branch master
    Changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
      deleted:    test.txt
    no changes added to commit (use "git add" and/or "git commit -a")

这时你有二种选择
    * 第一种确实从版本库中删除该文件 `git rm file`
    * 第二种恢复该文件 `git checkout  -- file`

### 远程仓库

#### 1. 远程仓库
这里我们选用github提供的免费远程仓库，下面介绍一下如何链接到github，注意这里没有讲如同步本地仓库和远程仓库，只是将如何链接到github
    
* 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：`ssh-keygen -t rsa -C "youremail@example.com"`,你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
* 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
![](http://www.liaoxuefeng.com/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)
* 为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了

#### 2. 链接远程仓库

  1. 第一步，在github上创建一个远程仓库，这个我就不赘述了。创建后我们得到一个空的仓库。
  2. 第二步，我们在本地添加一个远端的仓库
  `git remote add origin git@github.com:michaelliao/learngit.git`
    * origin:这个是远程仓库的名字，是git默认的，也可以改成别的
    * 记得更换的github仓库地址
  3. 第三步，我们将本地的内容推送至远端，`git push -u origin master` 
    * `git push`命令实际上是把当前分支同步到远端
    * 由于远端库是空的，我们加上`-u`参数，不仅会把本地仓库的当前分支内容推送至远端的同名分支，而且还会建立2个分支的关系。以后再推送或者拉取的时候就会简化命令。
    * 第四步，我们可以更新本地的内容，然后用`git push origin master`

#### 3. 从远程仓库克隆
  1. 使用`git clone yourgithubaddres` 即可从远端克隆一个仓库，在本生成一个同名的文件夹。
  2. git除了可以使用ssh协议外还可以使用https协议，缺点是每次都需要输入用户名及密码，要使用https协议，则仓库的地址就需要更换为https方式。

### 分支管理
分支就像平行宇宙一样，在软件版本管理中，类似于稳定版本、测试版本、超前版本等。

#### 1. 创建与合并分支
  1. 分支与HEAD的关系
    ![](http://www.liaoxuefeng.com/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)
    *  从上图中可以看出HEAD实际指向的是当前的分支master
    *  而分支master记录了当前仓库的状体
    *  我们用`cat .git/HEAD`查看HEAD的值
    ```
    $ cat .git/HEAD
    ref: refs/heads/master
    ```
    *  我们用`cat .git/refs/heads/master` 查看当前分支的值
    `9d9db4b1df6016d38b6d5d3db9dfd9543bef4cf3` 会得到一个hash值

    *  我们用`git log`查看历史记录，发现最后一次提交的状态的值为
    `9d9db4b1df6016d38b6d5d3db9dfd9543bef4cf3 更新远程仓库后的第一个存档`
    *  我们找到了master指向的位置，也找到了HEAD的指向实际状态
  
  2. 创建新的分支
    *.  `git checkout -b dev`创建一个新的分支dev并切换到当前分支,它相当于下述二个命令
    ```
    git branch dev  创建分支命令 
    git checkout dev 切换分支命令
    ```
    *.  我们可以用`git branch`查看当前工作的分支情况
    *.  如果这时候我们在dev分支上进行修改和提交就相当于下述情况
    ![](http://www.liaoxuefeng.com/files/attachments/001384908892295909f96758654469cad60dc50edfa9abd000/0)
  
  3. 分支合并
当我们切换的master分支的时候发现之前的修改都不见了，因为主分支的commit还是"更新远程仓库后的第一个存档"这个状态，如果我们想将主分支与dev分支的内容保持一致就使用分支合并命令。
`git merge dev`  这个命令用于合并指定分支到当前分支
    ```
    $ git merge dev
    Updating d17efd8..fec145a
    Fast-forward
     readme.txt |    1 +
     1 file changed, 1 insertion(+)
    ```
注意这个命令返回的信息中的`Fast-forward`,他说明这是一个快速的合并，他直接把master分支指向dev分支
  4. 删除分支
  我们使用`git branch -d dev`来删除dev分支

#### 2. 解决冲突
  1.  冲突的产生
  当二个分支对同一份文档的内容的修改产生了冲突后，如果我们进行分支合并操作，将会提示失败，提示内容如下：
  ```
  $ git merge feature1
  Auto-merging learnnote.md
  CONFLICT (content): Merge conflict in learnnote.md
  Automatic merge failed; fix conflicts and then commit the result.
  ```
  这将提示你手工解决冲突后，在进行合并
  2.  冲突的解决
  我们打开冲突的文件，发现git已经把重复的信息追加到文件的尾部了
```
  <<<<<<< HEAD       
    在这里也修改了文档
  =======
  当我们在某一个分支修改了文档，而又在另一个分支也同样修改了改文档，且修改的内容产生冲突时
  >>>>>>> feature1
```
  我们在看当前的状态信息：这里也提示你产生了冲突
```
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 2 commits.
    (use "git push" to publish your local commits)
  You have unmerged paths.
    (fix conflicts and run "git commit")

  Unmerged paths:
    (use "git add <file>..." to mark resolution)

          both modified:   learnnote.md

  no changes added to commit (use "git add" and/or "git commit -a")
```
  
  * **这里明确告诉你解决冲突的方式是先手工调整冲突的文档，然后再add，然后再commit**
  * **注意：手工调整这个词的意思是系统不会管你调整了那些，只要你add了系统就认为你调整完毕了**
  * 我们手工调整了冲突文件，那么本次合并及冲突解决了

  3. 冲突后的分支状态
  * 之前没有产生冲突的合并采用的都是快速合并，其分支状态一直是这个样子的
  ![](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)
  采用`git log --graph`查看就是一条直线
  * 而产生冲突后的分支状态 (实际上是不使用快速合并)的分支是这个样子的
  ![](http://www.liaoxuefeng.com/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0)
  采用`git log --graph --pretty=oneline`查看的样子
  ```
  $ git log --graph --pretty=oneline
  -   4eaff9bb60fcd7a2e479a28dc6d9688cedba315f  冲突解决完毕
  |\
  | * d1fb87206cacdc52b18406ff27b923a27390136c 这是在feature1上的第二次提交
  - | 9594c934cdbf50275c3f7fc288662c6cf1eb2f7c 这是在与feature1产生冲突的一个提交
  |/
  - 15ded06de2478497c4dae09663a7536d3353628d 这实在feature1上的第一次更新
  - 80588c667b9a4e478a6378c0e585072b3ae91d78 在dev分支上的第二次修改
  - 76c874235eb44abd6d4311472f0a0716f9cd3fd4 在dev分支上的首次修改
  ```
  * 这是后master分支已经指向解决冲突后的版本，而feature1分支还是原地未动。

#### 3. 分支管理策略

从上文我们知道Fast-forward模式合并分支，实际上是把master分支直接指向了dev分支，在删除分支后就会丢失分支信息，如果我们禁用这个功能，那么在合并的时候就会产生一个新的commit,那么就可以在历史信息中查看分支的走向。

  1. 禁用Fast-forward模式
  `git merge --no-ff branchname`
  使用这个模式后就会得到向上文一样的分支走向

  2. 分支策略
  
    * 一般来说master分支是非常稳健的，我们轻易不会去修改他 ，我们一般在dev分支进行开发，当代码足够稳健时再合并分支
    * 同时我们每个开发者都有一个自己的分支，再完成代码后不停与dev分支合并。
  ![](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

#### 4. bug分支
我们无时无刻不在解决BUG，git提供了我们随时可以建立bug分支的能力，在分支上解决问题并提交修改，可以快速的完成bug的修复。本节提出了几个新的特性。

  1. 状态保存
    * git提供保存当前工作区的指令，用来临时从当前的状态跳出去做其他事情，例如修复bug当操作完毕后再恢复工作区即可
    * git stash 是存储工作区的指令，存储后工作区会被清空。
  2. 工作区恢复
    * git stash list 查看存储现场
      * git stash app 恢复工作区，但是恢复后stash并不删除，你需要使用`git stash drop`删除stash
      * git stash pop 取回存储现场，并删除stash

#### 5. 分支删除
  * `git branch -d feature1` 如果你的分支并未合并，那么该命令将失效，提示你分支未合并
  * `git branch -D feature1` 是强制删除分支的命令
<<<<<<< HEAD
=======

#### 6.删除远程分支
  * git branch -a 查看远程分支
  * git push origin --delete <branchname> 注意这个命令不能删除github的默认分支，如果要删除请在github设置其他默认分支
>>>>>>> 88728074c3704c75241f2611ce2fb4d57cea006f
  
#### 6. 多人协作
当你从远程库克隆时，实际上git将把远程库的master分支与本地的master分支对应起来，并且远程仓库的名称默认是origin.
  1. 查看远程库
    * `git remote` 查看远程库信息，或者用`git remote -v`查看详细信息
  2. 推送分支
    * 把分支上的所有本地数据提交到远程库，推送时要指定本地分支。
    `git push origin dev`
  3. 抓取分支
    * 再克隆远程库后默认的情况下你只能看到master分支
    * 如果你想在DEV上进行开发，你就必须创建远程origin的dev分支到本地，使用`git branch -b dev origind/dev`命令
  4. 从远程仓库更新数据
  `git pull` 可以将远程从仓库的数据更新至本地
  5. 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name

### 标签管理
标签与分支类似就是某一个时刻的commit，只不过分支可以移动标签是固定不可移动的。
1. 建立标签
  * 切换到需要打标签的分支上
  * `git tag <tagname>` 将标签打到最新的commit上
  * 如果忘记打标签了，现在想补上，那么使用`git tag <tagname> commitid`
  * 使用`git show <tagname>`查看标签信息
  * 还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字`git tag -a <tagname> -m "this is my tag" commitid`
  * 还可以通过-s用私钥签名一个标签：`git tag -s v0.2 -m "signed version 0.2 released" commitid` 签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错。

2. 操作标签
  * 删除标签:`git tag -d <tagname>`
  * 推送远程标签：`git push origin <tagname>`,或者一次性推送所有未推送至远程的标签`git push origin --tags`
  * 远程删除标签：`git push origin :refs/tags/<tagname>`

### 配置git
1. 全局设置的配置方式

    * git的全局配置文件在~/.gitconfig文件内    
    * 可以使用`git config --global [option]`来定义全局配置

2. 忽略特殊文件
并不是所有的文件都需要放在仓库中，如软件运行过程中产生的中间文件，一些图标文件等，我们仅需要软件仓库(.git/的上级目录下)的根目录下编辑.gitignore，将需要忽略的文件放置其中即可。
    * 我们也可以通过在全局.gitconfig下设置命令，使得全部仓库都忽略以一些文件，可以参考其他的资料。
    * 注意编写了.gitignore不要忘记将其commit到版本库。否则下次无法使用的。

3. 

