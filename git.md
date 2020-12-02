## Git是什么？
Git是目前世界上最先进的分布式版本控制系统。

## Git常用命令
```
设置用户名和邮箱标识：
git config --global user.name “xxxxxx”
git config --global user.email “xxxxxxxx@xx.com”

git init	//初始化仓库
git init [project-name]		//新建一个目录，将其初始化为Git代码库

git add [file1] [file2] //添加指定文件到暂存区
git add [dir]		//添加指定目录到暂存区，包括子目录
$ git add .		//添加当前目录的所有文件到暂存区

git commit –m “说明”	//将文件提交到仓库
git status		//查看文件状态
git diff	//查看文件修改的内容

git mv a.txt b.txt 		//把a.txt改名为b.txt

git log	//查看历史记录
git log --pertty=online		//简略查看历史记录

git reset --hard HEAD^	//回退到上一个版本
git reset --hard HEAD 版本号	//回退到对应的版本号

cat xxx	//查看文件内容
git checkout -- xxxx	//丢弃工作区的修改（就是文件目录下的）

git remote add origin githubAddress		//链接远程仓库
git push -u origin master	//把本地仓库的分支master推送到github上去（第一次链接github的时候）
git push origin master	//第一次链接完成后的向github上提交代码

git clone githubAddress	//从github上克隆代码 

git checkout -b dev	//表示创建并切换dev
git checkout master	//切换到master分支
git branch 		//查看所有分支
git branch -d dev	//删除dev分支
git merge name		//合并name分支到当前分支



```

## 在windows上如何安装Git？
为Git是分布式版本控制系统，所以需要填写用户名和邮箱作为一个标识。
**注意：**git config --global 参数，有了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱。


## 如何操作？
### 创建版本库
什么是版本库？版本库又名仓库，英文名repository,你可以简单的理解一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件”还原”。
1. 通过命令 git init把这个目录变成git可以管理的仓库
2. 把文件添加到版本库中。
```
git init
git add README.md
git commit -m "first commit"
```
### 版本回退
git log命令显示从最近到最远的显示日志
显示的信息太多的话，我们可以使用命令 git log –pretty=oneline

#### 现在我想使用版本回退操作，我想把当前的版本回退到上一个版本，要使用什么命令呢？
使用如下2种命令，
第一种是：
git reset --hard HEAD^那么如果要回退到上上个版本只需把HEAD^改成 HEAD^^以此类推
那如果要回退到前100个版本的话，使用上面的方法肯定不方便，我们可以使用下面的简便命令操作：git reset --hard HEAD~100即可
第二种是：
git reset --hard 版本号
通过如下命令即可获取到版本号：git reflog

### 理解工作区与暂存区的区别

**工作区：**就是你在电脑上看到的目录，比如目录下testgit里的文件(.git隐藏目录版本库除外)。或者以后需要再新建的目录文件等等都属于工作区范畴。
　　**版本库(Repository)：**工作区有一个隐藏目录.git,这个不属于工作区，这是版本库。其中版本库里面存了很多东西，其中最重要的就是stage(暂存区)，还有Git为我们自动创建了第一个分支master,以及指向master的一个指针HEAD。
　　我们前面说过使用Git提交文件到版本库有两步：
　　第一步：是使用 git add 把文件添加进去，实际上就是把文件添加到暂存区。
　　第二步：使用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支上。
### Git撤销修改和删除文件操作
#### 撤销修改：
有如下几种方法可以做修改：
　　第一：如果我知道要删掉那些内容的话，直接手动更改去掉那些需要的文件，然后add添加到暂存区，最后commit掉。
　　第二：我可以按以前的方法直接恢复到上一个版本。使用git reset --hard HEAD^
但是现在我不想使用上面的2种方法，我想直接想使用撤销命令该如何操作呢？首先在做撤销之前，我们可以先用 git status 查看下当前的状态

#### 删除文件:
一般情况下，可以直接在文件目录中把文件删了，或者使用如上rm命令：rm [名称].txt，
如果我想彻底从版本库中删掉了此文件的话，可以再执行commit命令 提交掉，现在目录是这样的:

只要没有commit之前，如果我想在版本库中恢复此文件如何操作呢？
可以使用如下命令 git checkout -- [名称].txt



## 远程仓库

在了解之前，先注册github账号，由于你的本地Git仓库和github仓库之间的传输是通过SSH加密的，所以需要一点设置：
　　第一步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，、
再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果有的话，直接跳过此如下命令，如果没有的话，打开命令行，输入如下命令：

```
ssh-keygen  -t rsa –C "youremail@example.com"　

```
第二步：登录github,打开” settings”中的SSH Keys页面，然后点击“Add SSH Key”,填上任意title，在Key文本框里黏贴id_rsa.pub文件的内容
第三步：如何添加远程库？
　　现在的情景是：我们已经在本地创建了一个Git仓库后，又想在github创建一个Git仓库，并且希望这两个仓库进行远程同步，这样github的仓库可以作为备份，又可以其他人通过该仓库来协作。
　　首先，登录github上，然后在右上角找到“create a new repo”创建一个新的仓库。

```
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/jiabayou/demo.git
git push -u origin master

```
