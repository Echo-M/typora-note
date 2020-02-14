## git安装和环境配置

### 安装git

**两种安装方法：**

（1）直接下载安装包进行安装：

（2）用homebrew指令下载，需要事先安装homebrew。

我使用的是第一种下载安装包的方式来进行安装，可以在https://git-scm.com/downloads下载Git的安装包。

**安装好Git之后，配置用户名和邮箱，每次与Git交互都会使用；**

git config --global user.name “yourname”

git config --global user.email “youremail”

可以使用git config --list 指令查看配置信息

使用--global表示将上面的配置项设为全局使用，所有的项目都会默认使用这的配置信息。 



检查是否成功：**

git --version

**参考：**https://www.atatech.org/articles/117205

### aone上git的使用

**设置密钥，配置gitlab**

```CQL
//用自己的邮箱名称生成sshkey
ssh-keygen -t rsa -C "xxx.xxx@alibaba-inc.com" 

//查看id_rsa.pub文件，然后把公钥拷贝到gitlab的sshkey中，完成关联
cat ~/.ssh/id_rsa.pub 
```

如果之前已经有了，可以copy过来直接用，注意拷贝的内容是从ssh-rsa开始知道邮箱结束的全部内容~~

成功之后如下图所示

![image-20191015140324673](/Users/baola/Library/Application Support/typora-user-images/image-20191015140324673.png)

### 申请代码库权限

**渠道一：**

阿里内部的gitlab：http://gitlab.alibaba-inc.com/

进行搜索，会跳转到Aone

**渠道二：**

也可以直接在Aone上搜索：https://aone.alibaba-inc.com/

选【研发】→【代码】→【代码搜索】

### 生产环境git clone

一般来说，本地开发或线下环境，可以在个人账号下运行ssh-keygen，把生成的公钥保存到gitlab的ssh keys，就可以实现无密码git clone

而生产环境屏蔽了ssh-keygen，不能这么搞了。

搜到一篇文章[生产环境访问Maven,SVN,Gitlab等工具的方式说明](https://www.atatech.org/articles/40907?spm=a1z2e.8101737.webpage.dtitle4.26fb4f9bWmSAPF)

按照下面的格式，填上username和private_token，就可以了。

```CQL
git clone http://{username}:{private_token}@gitlab-sc.alibaba-inc.com/{group_name}/{project_name}.git
```

## git命令学习

### 创建一个新的repository

```CQL
1. 先在github上创建并写好相关名字，描述。
2. cd /hello−world   //到hello−world目录
3. git init  //初始化
4. git add .   //把所有文件加入到索引（不想把所有文件加入，可以用gitignore或add具体文件)
5. git remote add origin git@github.com:WadeLeng/hello−world.git  //增加到remote
6. git commit -m "first commit"  //提交到本地仓库，然后会填写更新日志( -m “更新日志”也可)
7. git push -u origin master
```

### 检出已有的代码

```CQL
git clone repository
git clone -b branch_name --recurse-submodules repository_url local_dir_name
git clone -b rtc --recurse-submodules http://gitlab.alibaba-inc.com/ARTC/speed.git xserver

// 拉取分支
git pull
```

### 查看分支

```java
// 本地
git branch
// 远程
git branch -r
// 查看所在分支
git status
// 查看所有分支（包括本地和远程）
git branch -a
```

### 提交更改

```java
// 查看commit的更改
git status

// 重命名文件、文件夹
git mv oldName newName

// 增加，删除文件
git add/rm filename

// 吧所有更改加入git
git add .

// commit到本地仓库
git commit -m "describe"

// push到远端
git push
```

### 合并分支

将分支`dev`合并到当前分支中，自动进行新的提交：

```shell
$ git merge dev
```

将分支`maint`合并到当前分支中，但不要自动进行新的提交：

```shell
$ git merge --no-commit maint
```

当您想要对合并进行进一步更改时，可以使用此选项，或者想要自己编写合并提交消息。应该不要滥用这个选项来潜入到合并提交中。小修补程序，如版本名称将是可以接受的。

### git merge一个指定文件

**合并某个分支的指定文件到另一个分支**

git里面的merge是全merge ，没有单个文件merge。

要实现一个文件的merge ，可以使用git checkout 这个命令



git checkout xxxx（分支名）  xxxx（文件名）

这个命令是覆盖的意思，是说把另一个分支的文件覆盖到当前的分支上，

所有，最好不要在master上面操作，可以建立一个临时的分支，然后，commit。

在merge到master分支上，这样就实现了单个文件的merge。



当然，这个功能还有一个作用，就是文件的回退，例如你改了这个文件，

然后你想变回和服务器一样的文件，那么你可以用下面的命令。

git checkout HEAD  xxxx（文件名）

就会回退到服务器的版本文件一直，也是覆盖功能，就是把服务器的文件取下来，覆盖到本地了。

git是用HEAD这个指针来控制文件的。

### 删除分支

```java
// 删除本地分支
git branch -d <BranchName>
```



### 创建分支并和远程分支建立映射

```java
// 在本地新建分支x，并自动切换到该本地分支x。
// 采用此种方法建立的本地分支会和远程分支建立映射关系。
git checkout -b 本地分支名x origin/远程分支名x

// 在本地新建分支x，但是不会自动切换到该本地分支x，需要手动checkout。
// 采用此种方法建立的本地分支不会和远程分支建立映射关系。
git fetch origin 远程分支名x:本地分支名x

// 查看本地分支和远程分支的映射关系
git branch -vv

// 建立当前分支与远程分支的映射关系，有2种方法：
git branch -u origin/分支名
git branch --set-upstream-to origin/分支名

// 撤销当前分支与远程分支的映射关系
git branch --unset-upstream
```



### git丢弃本地修改的所有文件（新增、删除、修改）

下面也这话的最终结果：本地所有修改被丢弃，最终和远端一致

```cmd
git checkout . && git clean -xdf
```

```cmd
git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。
git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改

git clean -df #返回到某个节点
git clean 参数
    -n 显示 将要 删除的 文件 和  目录
    -f 删除 文件
    -df 删除 文件 和 目录
```
