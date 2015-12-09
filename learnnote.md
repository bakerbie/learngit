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

### 1. 安装git
这里只介绍windows下的安装，msysgit是Windows版的Git，从 http://msysgit.github.io 下载，然后按默认选项安装即可。
### 2. 版本库的建立
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

### 查看当前的状态
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

    Changes not staged for commit:  说明有未向缓存区提交的文件
      (use "git add <file>..." to update what will be committed) 提交命令说明
      (use "git checkout -- <file>..." to discard changes in working directory) 放弃修改名利

        modified:   learnnote.md   文件名称

**这里有几个命令需要留意**
    1. `git checkout -- <filename>` 这个命令是取消工作区的修改，修改后与暂存区一致 
    2. `git rm --cachedd <filename>` 
    这个命令是删除暂存区的文件，如果暂存区的文件和工作区的文件不同，系统会提示你，如果想要强制删除需要`-f`选项
