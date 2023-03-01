this is a test.
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

