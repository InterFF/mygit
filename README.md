> 不重复造轮子，只是记录简单实用的几个命令。类似词典功能，供日后查询使用。

TODO:dart:
- [ ] 合并代码技巧
- [ ] 提交代码技巧



# 命令

## About git Version
   ```shell
   $ git --version
   ```

## 创建仓库

1. 本地自建一个git 仓库

   ```shell
   //方式一,已存在仓库
   $ cd respositoryName
   $ git init
   
   //方式二，同级目录，存在，则初始化，不存在，则创建一个
   $ git init respositoryName
   
   ```

2. 从远程克隆

   ```shell
   //方式三，copy别人的仓库
   $ git clone <url>
   
   //or 重新命名
   $ git clone <url> newName
   
   //clone 名字叫dev的远程分支（非默认）
   $ git clone -b dev <url>
   ```

## 远程仓库(remote)

一个远程仓库代表一台主机，一个仓库下可以有多个分支。



1. 关联远程仓库

   ```shell
   //本地有仓库,shortname一般是origin,可以自定义
   $ git remote add <shortname> <url>
   ```

2. 查看远程仓库

   ```shell
   //列出所有
   $ git remote
   
   //or 更详细
   $ git remote -v
   
   //查看某一个，可以看到此远程仓库下的所有分支
   $ git remote show <shortname>
   ```

3. 重命名远程仓库shortname

   ```shell
   //重命名,本质是把远程仓库对应的本地的索引重新命名
   $ git remote rename shortName newShortName
   ```

4. 解绑远程仓库

   ```shell
   //解绑
   $ git remote remove shortName
   ```

5. 提交到远程仓库

   ```shell
   //第一次提交
   //语法：git push <远程主机别名> <本地分支名>:<远程分支名>
   $ git push -u origin master:master
   //如果本地分支名和远程分支名相同，可以省略冒号和远程分支名 ，语法：git push <远程主机别名> <本地分支名>
   $ git push -u origin master
   
   //之后的提交
   $ git push
   ```

   其实，执行添加了-u 参数的命令 git push -u origin master就相当于是执行了

   git push origin master 和
   git branch --set-upstream master origin/master。

   所以，在进行推送代码到远端分支，且之后希望持续向该远程分支推送，则可以在推送命令中添加 -u 参数，简化之后的推送命令输入。

   简单来说，带上`-u` 参数其实就相当于记录了push到远端分支的默认值，这样当下次我们还想要继续push的这个远端分支的时候推送命令就可以简写成`git push`即可。

6. 从远程仓库抓取

   ```shell
   //origin是别名， 必须注意 git fetch 命令只会将数据下载到你的本地仓库——它并不会自动合并或修改你当前的工作
   $ git fetch origin
   
   //比较
   $ git diff origin/master
   
   //合并到本地分支
   $ git merge origin/master
   
   //or 将数据下载到你的本地仓库并且自动合并
   $ git pull origin master
   ```

   

## 版本回退

![image-20240425140535294](./img/file_st.png)

1. 修改了，但是还没有add, 覆盖本次修改【unmodified】or 【untracked】

   ```shell
   //恢复某个文件,也可以是多个
   $ git restore 1.md 2.md
   
   // 当前目录下所有文件，都恢复原状
   $ git restore .
   ```

2. add后，还没有commit, 撤回到add 前一步【unstaged but modified】

   ```shell
   // 恢复某个文件，也可以是多个
   $ git restore --staged 1.md 2.md
   
   // 当前目录下所有文件，都恢复原状
   $ git restore --staged .
   ```

3. commit 后，还没有push,撤回到commit前一步,即回到【staged】

   ```shell
   $ git reset --soft HEAD^	
   ```

4. commit 后，还没有push,撤回到add 前一步,保留修改的内容【unstaged but modified】

   ```shell
   $ git reset HEAD^	
   ```

5. commit 后，还没有push,撤回到未修改的状态，不保留修改的内容【unmodified】or 【untracked】

   ```shell
   $ git reset --hard HEAD^
   ```

6. 以上只是针对本地仓库，对于有远程仓库的，commit 后，已经push,一同改变远程仓库和本地的内容，使用revert命令
   ```shell
   //撤销最近一次变更，就是这么简单，本地的也一并撤销了
   $ git revert HEAD
   $ git push origin master
   
   ```

7. commit 很久以后，想要删除之前的某一次提交，后面的提交不动
   ```shell
   //反做指定的提交id
   $ git revert commit_id
   
   //然后解决冲突,然后提交
   $ git add .
   
   $ git commit
   
   $ git push
   ```

8. 修改最后一次提交

   修改你最近一次提交可能是所有修改历史提交的操作中最常见的一个。 对于你的最近一次提交，你往往想做两
   件事情：简单地修改提交信息， 或者通过添加、移除或修改文件来更改提交实际的内容。

   ```shell
   //修改注释
   $ git commit --amend
   
   //提交注释不必修改，你只需作出更改，暂存它们
   $ git commit --amend --no-edit
   ```

   

## 标签管理(tag)

1. 看标签 `git tag`

    ```shell
    //列出标签
    $ git tag

    //搜索标签
    $git tag -l "v0.2.*"

    //查看标签
    $git show v0.1.0
    ```

2. 新增标签 `git tag <tag-name>`

    ```shell
    //新增当前head
    $ git tag v0.2.0

    //以前的
    $ git tag v0.1.0 commid

    //-a 附注标签（annotated）,GPG签名并验证，-m 选项指定了一条将会存储在标签中的	信息。 如果没有，Git会启动编辑器要求你输入信息
    $ git tag -a v0.1.0 commid -m "release v0.1.0"

    //私钥签名，前提安装 gpg
    $ git tag -s v0.1.0 commid -m "signed release v0.1.0"

    //远程提交
    $ git push origin v0.2.0

    //远程提交全部
    $ git push origin --tags
    ```

3. 删除标签 `git tag -d v0.1.0`

    ```shell
    //删除本地
    $ git tag -d v0.1.0

    //删除远程
    $ git push orgin :v0.1.0

    //or
    $git push origin --delete v0.1.0
    ```

4. 切换到tag版本

    ```shell
    $ git checkout v0.1.0
    ```

## 设置别名（alias）

1. 新增别名 `git config --global alias.<alias> <command>`

   ``` shell
   //本仓库适用，如果当前仓库设置别名，会覆盖全局设置，一般本仓库不设置，仅设置全局
   $ git config alias.st status
   
   //全局适用,一般默认设置全局
   $ git config --global alias.st status
   
   //组合命令别名，加单引号
   $ git config --global alias.last 'log -1'
   //or 双引号
   $ git config --global alias.last "log -1"
   ```

2. 查看别名 `git config alias.name`

   ```shell
   // 查看所有
   $ git config --global --list | grep alias
   
   // 指定搜索 叫`st`的别名
   $ git config --global alias.st
   ```

3. 修改别名

   ```shell
   // 此处同新增别名，参考1
   ```

4. 删除别名

   ```shell
   // 参考5
   ```


5. 批量编辑 （增删改）

   ```shell
   //系统所有用户
   $ git config --system -e
   
   //全局单用户
   $ git config --global -e
   
   //本仓库
   $ git config -e
   ```
优先级 local > global > system
## 临时保存 (stash)

应用场景：当一个分支的开发工作还未完成时，却要切换到其他的分支去。如果什么都不做，那么就会导致本次修改会带到其他分支去，进而影响到其他分支的代码。此时，为了避免这样的事情发生，stash命令就派上用场了。使用stash命令，临时保存此次还没有完成的修改到git栈中，然后切换到其他分支去，完事了再切回来，从git栈中取出刚才保存的内容，继续上次未完的开发。


1. 新增（入栈）

    ```shell
    //保存本次修改
    $ git stash

    //保存本次修改，并添加注释，方便查找
    $ git stash -m "this is a test"
    or
    $ git stash save "this is a test"
    ```

2. 查看

    ```shell
    // 查看所有
    $ git stash list

    // 查看某一个stash修改的文件有哪些，显示文件名，不加序号n,默认最近一个
    $ git stash show n

    // 查看某一个stash修改的哪些文件的哪些内容，跟diff差不多。不加序号n,默认最近一个
    $ git stash show n -p

    ```

3. 取出

    ```shell
    // 取出并删除，出栈用法.指定某一个，不加序号n,默认最近一个
    $ git stash pop n

    //取出不删除,搭配drop使用.指定某一个，不加序号n,默认最近一个
    $ git stash apply n
    ```

4. 删除，搭配apply使用

    ```shell
    // 指定某一个，不加序号n,默认最近一个
    $ git stash drop n
    
    //全部删除
    $ git stash clear
    ```

## 分支管理（branch）

1. 新增

   ```shell
   //新增,若无commitId，则默认基于当前分支
   $ git branch branchname commitId
   
   //新增并且切换到新的分支,若无commitId，则默认基于当前分支
   $ git checkout -b branchname commitId
   
   //拉取远程分支并创建本地分支，在本地新建分支localbranchname，并自动切换到该本地分支localbranchname，建立的本地分支会和远程分支建立映射关系
   $ git checkout -b localbranchname remotebranchname
   
   //举例子
   $ git checkout -b de origin/dev
   ```

2. 查看

   ```shell
   //查看
   $ git branch
   
   //筛选 名字
   $ git branch -l "feat*"
   
   //筛选 已合并
   $ git branch --merged
   
   //筛选 未合并
   $ git branch --no-merged
   
   ```

3. 修改

    ```shell
    //newBranch存在不覆盖
    $ git branch -m oldBranch newBranch
    
    //newBranch存在覆盖
    $ git branch -M oldBranch newBranch
    ```
4. 删除

    ```shell
    //仅仅适用于已经合并的分支
    $ git branch -d branchname
    
    //未合并的使用此命令
    $ git branch -D branchName
    
    //删除远程分支
    $ git push origin --delete branchname
    ```

## 远程仓库别名和分支名

“origin” 并无特殊含义，远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。 

同时 “master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用， 

“origin” 是当你运行 git clone 时默认的远程仓库名字。下面讨论怎么修改它们。



1. 分支的别名修改

    ```shell
    //参考分支管理 3.修改
    $ git branch -M master main
    ```

2. 远程仓库别名的修改

    ```shell
    //初始命名
    $ git clone -o shortname <url>
    
    //事后修改
    $ git remote rename oldshortname newshortname
    ```

3. 远程仓库新建分支ft_newbranch

   ```shell
   //本地有一个分支，远程看不到，那么将本地分支添加到远程仓库就能看到了
   $ git push origin ft_newbranch
   ```

   

## 跟踪分支

如果在一个跟踪分支上输入 git pull，Git 能自动地识别去哪个服务器上抓取、合并到哪个分支。这就要得益于跟踪分支。

1. 查看

    ```shell
    //查看设置的所有跟踪分支
    $ git branch -vv
    
    
    //统计最新的
    $ git fetch --all;git branch -vv
    ```

2. 创建远程跟踪分支

   ```shell
   //1.创建一个本地serverfix分支，并且跟踪远程分支serverfix, 别名origin
   $ git checkout --track origin/serverfix
   
   //or 设置本地分支名 localbranchname
   $ git checkout -b <localbranchname> <remote>/<remotebranchname>
   ```

3. 修改

   ```shell
   //重新指向origin/serverfix
   $ git branch -u origin/serverfix
   ```

4. 删除

    ```shell
    //要删除当前分支的上游
    $ git branch --unset-upstream
    ```



## 搜索代码

Git 提供了一个 grep 命令，你可以很方便地从提交历史、工作目录、甚至索引中查找一个字符串或者正则表达式。


```shell
//搜索包含string的文件
$ git grep string

//显示行号
$ git grep -n string

//显示山下文
$ git grep -p string
```

## 比较（diff）

```shell
//比较「暂存区」与「工作区」之间的差异。
$ git diff

//比较「给定提交 ID」与「工作区」的差异
$ git diff commitId

//比较「暂存区」与「给定提交 ID」的差异,如果省略 commit，那么，就是默认指 HEAD
$ git diff --cached commit
or
$ git diff --staged commit

//比较指定的两次提交 「commit1」与 「commit2」的差异,`commit1=a(old),commit2=b(new)`
$ git diff commit1 commit2
```

高级用法
```shell
//显示有多少行发生变化，简洁的展示差异,不显示具体内容
$ git diff --stat

//限制路径-目录
$ git diff --stat HEAD~ HEAD src

//文件
$ git diff --stat HEAD~ HEAD src/test.txt

//打补丁
$ git diff commit1  commit2 > patch

//应用补丁
$ git apply patch
```

## 日志（log）

1. 提交日志

记录提交版本，分支变迁，提供详细的日志信息，用来查询
```shell
//直接查看
$ git log

//所有分支
$ git log --all

//最近n条
$ git log -n

//单行显示
$ git log --online

//树图
$ git log --graph

//或者自定义
$ log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

//查看每次提交的日志的补丁，跟diff差不多
$ git log -p
```
2. 命令执行日志

用来“时空穿梭”
```shell
$ git reflog
```

## Credential Storage

```shell
// cache for 15 mins
$ git config --global credential.helper cache

// --timeout n ,unit second
$ git config --global credential.helper cache --timeout 1000

// --file [path],helper whith a custom file name
$ git config --global credential.helper 'store --file ~/.my-credential'
```

# 技巧

1. 刚提交了代码，发现问题没完全解决，再次修改，针对的是同一个问题，不想要产生2条提交记录，怎么办？

   ```shell
   $ git add .
   $ git commit --amend --no-edit
   ```

   

2. 正在A分支上开发，还没有提交，功能开发了一半。突然来了一个紧急任务，要切换到B分支，此时如果切换分支，A分支上修改的代码也会同步过去，想要在未修改的分支上进行开发，并且不想丢弃开发一半的功能，怎么搞？

   ```shell
   //A分支暂存起来，
   $ git stash
   
   //切换到其他分支
   
   //再切换回来A分支,就可以继续之前中断的动作了
   $ git stash pop
   
   ```

   

3. 我们本地的数据库和测试环境的数据库配置是不一样的，但是在项目开发中每次提交过程中忽略数据库配置文件。怎么做呢？

   1. 新增忽略文件

      ```bash
      //这里忽略READMD.md文件
      $ git update-index --assume-unchanged README.md 
      ```

   2. 查看被忽略的文件

      ```bash
      //查看
      $ git ls-files -v | grep '^h'
      h README.md //结果都是以h开头的
      ```

   3. 恢复跟踪

      ```bash
      //恢复跟踪
      $ git update-index --no-assume-unchanged   README.md  //恢复跟踪
      
      //恢复所有的跟踪
      $ git ls-files -v | grep '^h' | awk '{print $2}' |xargs git update-index --no-assume-unchanged  
      ```

4. 多账号切换开发，假设你有两个账号，平时一直都是用的主账号，突然有一个项目要用一个临时账号test，该怎么做？
   两步达到目的，第一：
   修改本项目的user和email，也就是local级别的配置，位于项目根目录下 .git/config 中。此处commit有用
   ```bash
   
   [user]
   name = test
   email = test@mail.com
   
   ```

   第二步，修改远程url,此处push有用

   ```bash
   	[remote "origin"]
   	url = http://test:123456@git.github.com/test/myproject.git
   	fetch = +refs/heads/*:refs/remotes/origin/*
   ```

   以上只针对https方式有用。

5. 远程分支默认是master, 你clone 远程仓库代码后，发现不是你想要的，你想要dev分支的代码，怎么办？

   ```bash
   // 参考分支管理，新建分支一节
   $ git checkout -b dev origin/dev
   ```

6. 验证SSH是否可用?

   ```bash
   $ ssh -T git@github.com
   Hi InterFF! You've successfully authenticated, but GitHub does not provide shell access. //返回这个信息就证明连接正常
   ```

   


# 提交流程

对于多人协作开发项目，可遵守的开发流程有N种。

## 一般协作模式

1. clone远程(origin)分支到本地，假如远程分支为master

   ```bash
   //参考第一节 远程仓库
   $ git clone <url>
   ```

2. 建立一个用于开发的分支dev，在dev上开发

   ```bash
   $ git checkout -b dev
   ```

3. dev提交代码

   ```bash
   $ git add .
   $ git commit -m 'first commit'
   ```

4. 切换master，拉取远程代码

   ```bash
   $ git co master
   $ git fetch origin
   
   //也可查看改动
   $ git diff origin/master --stat
   ```

5. 合并远程

   ```bash
   $ git merge origin/master
   //若有冲突，则解决，提交，然后进入下一步
   //若无冲突，则直接进入下一步
   ```

6. 合并dev

   ```bash
   //当前还在master分支，可比较本地修改合远程差异
   $ git diff dev --stat
   
   //合并
   $ git merge dev
   //若有冲突，则解决，提交，然后进入下一步
   //若无冲突，则直接进入下一步
   ```

7. 推送远程

   ```bash
   //第一次推送
   $ git push -u origin master
   
   //以后推送
   $ git push origin master
   ```

8. 切换dev，合并master

   ```bash
   $ git checkout dev
   //master已经是解决完冲突的，所以可以放心合并
   $ git merge master
   ```

9. 重复步骤2





------

官方文档 [progit](https://git-scm.com/book/en/v2)：权威，深刻

入门教程[尚硅谷Git教程](https://www.bilibili.com/video/BV1vy4y1s7k6): 入门级，简单实用

入门神器[图形教程](https://learngitbranching.js.org/?locale=zh_CN)：了解git原理，简单，明了，小白必看

提交错误的解决办法 [Oh Shit, Git!](https://ohshitgit.com/)：关键时刻能救命

[如何写好 Commit Message](https://chris.beams.io/posts/git-commit/) : 写出高可读的提交日志

[How to explain git in simple words](https://smusamashah.github.io/blog/2017/10/14/explain-git-in-simple-words)：用通俗易懂的语言解释git

[Git命令git update-index --assume-unchanged，忽略不想提交的文件（忽略跟踪）](https://www.cnblogs.com/wt645631686/p/10007328.html)

[Git指定文件不更新](https://blog.csdn.net/LittleGlassHeart/article/details/120459341)
