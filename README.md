this is a test.



## 版本回退

1. 修改了，但是还没有add, 覆盖本次修改

   ```shell
   //恢复某个文件,也可以是多个
   $ git restore 1.md 2.md
   
   // 当前目录下所有文件，都恢复原状
   $ git restore .
   ```

2. add后，还没有commit, 撤回到add 前一步

   ```shell
   // 恢复某个文件，也可以是多个
   $ git restore --staged 1.md 2.md
   
   // 当前目录下所有文件，都恢复原状
   $ git restore --staged .
   ```

3. commit 后，还没有push,撤回到add 前一步,保留修改的内容

   ```shell
   $ git reset HEAD^	
   ```

4. commit 后，还没有push,撤回到未修改的状态，不保留修改的内容

   ```shell
   $ git reset --hard HEAD^
   ```

5. commit 后，已经push,改变远程仓库内容,先reset本地内容，然后强推到远程
   ```shell
   //此种情况，必须是独有的仓库，也就是说参与者只能是自己，没有其他人参与
   $ git push -f origin master
   ```
6. commit 很久以后，想要删除之前的某一次提交，后面的提交不动
   ```shell
   //反做指定的提交id
   $ git revert commit_id
   
   //然后解决冲突,然后提交
   $ git add .
   
   $ git commit
   
   $ git push
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

//-a带注释的,不加-m后果自负
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
```

4. 切换到tag版本

```shell
$ git checkout v0.1.0
```

## 设置别名（alias）

1. 新增别名 `git config --global alias.<alias> <command>`

   ``` bash
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

   ```bash
   // 查看所有
   $ git config --global --list | grep alias
   
   // 指定搜索 叫`st`的别名
   $ git config --global alias.st
   ```

3. 修改别名

   ```bash
   // 此处同新增别名，参考1
   ```

4. 删除别名

   ```bash
   // 参考5
   ```


5. 批量编辑 （增删改）

   ```bash
   //全局
   $ git config --global -e
   
   //本仓库
   $ git config -e
   ```

## 临时保存 (stash)

应用场景：当一个分支的开发工作还未完成时，却要切换到其他的分支去。如果什么都不做，那么就会导致本次修改会带到其他分支去，进而影响到其他分支的代码。此时，为了避免这样的事情发生，stash命令就派上用场了。使用stash命令，临时保存此次还没有完成的修改到git栈中，然后切换到其他分支去，完事了再切回来，从git栈中取出刚才保存的内容，继续上次未完的开发。


1. 新增（入栈）

  ```sh
  //保存本次修改
  $ git stash

  //保存本次修改，并添加注释，方便查找
  $ git stash -m "this is a test"
  or
  $ git stash save "this is a test"
  ```

2. 查看

  ```sh
  // 查看所有
  $ git stash list

  // 查看某一个stash修改的文件有哪些，显示文件名，不加序号n,默认最近一个
  $ git stash show n

  // 查看某一个stash修改的哪些文件的哪些内容，跟diff差不多。不加序号n,默认最近一个
  $ git stash show n -p

  ```

3. 取出

  ```sh
  // 取出并删除，出栈用法.指定某一个，不加序号n,默认最近一个
  $ git stash pop n

  //取出不删除,搭配drop使用.指定某一个，不加序号n,默认最近一个
  $ git stash apply n
  ```

4. 删除，搭配apply使用

  ```sh
  // 指定某一个，不加序号n,默认最近一个
  $ git stash drop n

  //全部删除
  $ git stash clear
  ```

   
