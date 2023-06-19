# 1.官方参考

# 2.逐步说明
## 2.1 将本地(未绑定任何git项目)代码通过git命令上传到github的流程
首先在项目根目录打开命令行或者直接打开git-bash转到项目根目录下 
### 1.创建本地仓库
`$ git init ` 初始化本地仓库

> 如果init错了，可以通过 rm -rf .git 去除掉配置，重新安装
> 
### 2. 将本地文件提交上去
`$ git add --all` 将需要更新的整个文件加到本地仓库缓存中
或者
`$ git add . ` 将项目文件添加到跟踪列表
或者
`$ git add ./dev/ ./mode/` 

> 如果要过滤某类文件，可以编辑.gitignore文件`$vim .gitignore`，写入如下指令：
> ```shell
> *.pyc
> *.sqlite3
> *.DS_Store

`$ git commit -m [分支]` 有'分支'就用推到该‘分支’，没有，就新建‘分支’

将变化后的文件提交到本地仓库，一般在后面加上一些标注，表示本次的变动信息。

### 3.创建好本地仓库后，开始上传到github

```diff
+ 在github上手动创建一个新的respository,这时会返回一个远程git仓库链接XXX，如http://.../XX.git
```
`$ git remote add origin [分支]` "xxx"是你得到的链接url，remote是指远程git仓库

`$ git push -u origin [分支]` 将本地仓库推送到远端，-u表示的是让文件以流的方式上传

刷新github页面后可以看到本地git仓库已经成功上传。


**如果需要想要推送的是一个现有仓库的新分支（remote不存在的分支），应该怎么办？**
在本地创建新分支，然后推送的远端<br>
`git checkout -b 1.0.1` #检测有没有分支1.0.1，如果没有就创建，再切换到该分支下面<br>
`git branch` #查看当前分支情况，（*）表示该分支时当前分支<br>
`git push -u origin 1.0.1` #其中origin已经被指定为上文的git地址<br>

## 2.2 git命令合并分支代码
**对于复杂的系统，我们可能要开好几个分支来开发，那么怎样使用git合并分支呢？**<br>
合并步骤：
- 1、进入要合并的分支（如开发分支合并到master，则进入master目录）<br>
`$ git checkout master` #切换当前分支到master分支<br>
`$ git pull origin master`<br>
- 2、查看所有分支是否都pull下来了<br>
`$ git branch -a`<br>
- 3、使用merge合并开发分支<br>
`$ git merge [分支]`<br>
- 4、查看合并之后的状态<br>
`$ git status #会显示该分支提交了多少commits`<br>
- 5、有冲突的话，通过IDE解决冲突。 <br>
- 6、解决冲突之后，将冲突文件提交暂存区<br>
`$ git add` 冲突文件<br>
- 7、提交merge之后的结果<br>
`$ git commit`<br>
如果不是使用git commit -m "备注" ，那么git会自动将合并的结果作为备注，提交本地仓库；<br>
- 8、本地仓库代码提交远程仓库，完成更新<br>
`$ git push origin master` git将分支合并到分支，将master合并到分支的操作步骤是一样的。<br>
也可以用`git checrry-pick commitid`合并<br>

## 2.3 删除远程分支
- 方法1. 推送一个空分支到远程分支，其实就相当于删除远程分支：
```shell
$ git push origin:1.0.1
```
- 方法2. 推送一个空分支到远程分支，其实就相当于删除远程分支：
```shell
$ git push origin --delete 1.0.1
```
## 2.4 设置或删除git配置信息
```shell
$ git config --global http.proxy 'socks5://127.0.0.1:13659' #新增http.proxy设置项
$ git config --global --list #查询git config内容
>user.name=芒鞋
>user.email=mangxie.wg@alibaba-inc.com
>core.autocrlf=input
>http.proxy=socks5://127.0.0.1:13659

$ git congig --global --unset http.proxy #删除http.proxy设置项
```

========================分界线========================

## 上传本地git仓库代码时可能遇到的一些问题：
- 1.如何修改原有的remote地址

`git remote set-url origin [你自己创建的远程仓库]` 或者可以直接在.git文件夹里面的config文件中修改

- 2.如何解决I don't handle protocol 'xxx'

这个问题翻译过来就是“无法处理xxx协议”的意思，说明你的远程仓库url有问题，应该检查有没有写错

## 2.5 删除git远程文件
在git中可以用git rm命令删除文件（删除远程仓库文件）
```shell
git clone 仓库地址
git add .
step1: git rm 文件 //本地中该文件会被删除
step2: git rm -r 文件夹 //删除文件夹
step3: git commit -m '删除某个文件'
step4: git push （origin master）
```
上面的方法会把对应的本地文件也删除掉，如果不想把本地文件删除，只把缓存区中的对应部分删除，则加上--cache
```shell
git rm --cached 文件 //本地中该文件不会被删除
git rm -r  --cached  文件夹 //删除文件夹
```
完整版本：
```shell
git clone 仓库地址
git add .
 
 
step1: git rm --cached 文件     //本地中该文件不会被删除
step2: git rm -r  --cached  文件夹 //删除文件夹
 
step3: git commit -m '删除某个文件'
step4: git push （origin master）
```
