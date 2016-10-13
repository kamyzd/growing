## 一、Git 的诞生和特点

接触 Git,首先就要知道一个问题,啥四 Git 呀,Git 能干哈呀。Git 是一个版本控制系统,说的高大上一点就是 VCS(version control system)。源于大家对代码或者其他文件或文档的修改和痛苦管理,就有人搞了这个玩意儿,所 以呀,还是牛多,想起自己手动备份文件 V2016-9-1 之类的,真是 low 的一逼。 

Git 是 Linus 老爷子(就是那个创造 Linux 的神)搞的,他一开始并不想在这方面下功夫,都是用别人的东西管理 Linux的代码,不过老爷子有个准则就是开源、 免费,所以就用 BitKeeper(貌似只对老爷子免费)。无奈 Linux牛太多了,不安分守己地用,老是破解人家的协议,人家怒了,很刚,威胁说收回免费使用权。然而老爷子更刚,花了两周写了 Git(TMD,给我两年我都搞不出来),然后Git就火了,火到丧心病狂,火到六亲不认。
那么 Git 都有什么优势呢,也就是说,咋就这么火呢?且听我慢慢道来:

#### 分布式
说到分布式,就不得不提一下集中式版本控制系统。在集中式版本控制系统中,版本库是放在中央服务器的,大家都是先从中央服务器拿到最新的版本,然后在自己的电脑上搞,搞完事儿再提交到中央服务器。用这种的真的好烦,尤其 是中央服务器上西天的时候。那么分布式呢,就是木有中央服务器这个东西,直接每个人电脑上都有完整的版本库,大家即使是对同一个文件做修改,也是可以的,只需要把自己的修改推送给对方就行。实际上,分布式版本控制系统中也有个类似中央服务器的电脑,不过这只是为了推送更加方便而已,没有它也是 ok 的。

#### 性能可靠
一方面是能够支撑多人开发的规模要大,另一方面要能保证足够的速度。就拿Linux来说,每个版本的更新都是数以千计的人的共同努力,如果不能很好地支持很多的开发人员,将非常容易碰到性能瓶颈。还有就是木有速度的保证,开 发效率就受到很大的制约。

#### 保证完整性和可靠性
由于它是分布式的版本控制系统,所以就需要能够绝对保证数据的完整性和 可靠性。在 Git里面,用安全散列函数(SHA1)来命名和识别数据库中的对象。

#### 强化责任
很简单,就是 Git 可以对一切的改动进行责任跟踪

#### 不可变性
就是说,Git 版本库中存储的所有数据对象都不可变,即使重建一个一模一样的数据对象,原来的数据对象也不会被删除。Git 保证的是整个历史。

#### 原子事务
原子事务的特点是不可分割,要么执行,要么不执行。这样就使得 Git 每一个执行过的操作都是有效的,不会使版本数据库陷入部分改变或者破损的状态。

#### 支持基于分支的开发
这是 VCS 的共同特点,基本所有的 VCS 都支持分支,并且还要实现所有分支 的合并。
完整的版本库
这个好理解,这个就是分布式带来的好处,每个开发人员的电脑上都有一份 完整的版本库。

## 二、Git 入门

### 1. Git 的安装
一般说到 Git 的安装,就会有 Git 在 Linux 上的安装、在 Windows 上的安装、在 Mac 平台上的安装,可是我只会说关于 Linux 的,想在 Windos 和 Mac 上 做研究?玩蛋去吧。

Linux 比较好的一点是,你可以输入 git,看一下系统有木有安装 git,就 像下面这样:
```
$ git
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
```

Linux 除了告诉我们没有安装之外,还可以告诉我们怎样安装。
Debian/Ubuntu Git 安装命令为:
```
$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
$ apt-get install git-core
$ git --version
git version 1.8.1.2
```

Centos/RedHat 安装命令为:
```
$ yum install curl-devel expat-devel gettext-devel \ openssl-devel zlib-devel
￼￼￼￼￼￼￼$ yum -y install git-core
$ git --version git version 1.7.1
```
￼￼
### 2. Git 的基本使用
#### 了解 Git 命令
安装了 Git 后,输入 git,Git 就会不带任何参数地列出它的选项和最常用 的子命令。如果想得到一个完整的 git 子命令列表,可以输入 git help –-all。

#### 创建初始版本库
以个人网站开发为例,在~/public_html 目录创建个人网站,并把它放到 Git 版本库中。以下的操作时新建一个目录,并在 index.html 中放一些简单内容。

```
$ mkdir~/public_html
$ cd ~/public_html
$ echo 'This is my website'> index.html
```

然后输入 `git init`,将`~/public_html`或者任何目录转化为 Git 版本库向版本库里添加文件
`git init`命令是创建一个新的 Git 版本库,初始化后的 Git 版本库都是空
的,为了管理内容,必须很清晰准确地将内容放到版本库中。
使用`git add 文件名`,可以将文件添加到版本库中。可是有时候目录中会有很多文件,一个一个添加太过繁琐了,这时使用`git add .` 来将当前及子目录中的文件都添加到版本库中。 不过上面的操作只是暂存了这个文件,还需要进行提交操作,才可以把文件
真正地添加到版本库中。这时就用到`git commit`这个命令。如果没有配置提交 作者信息,则用这样一条完全限定的`git commit`命令(必须提供日志消息和作 者)来进行提交。
```
$ git commit -m “Initial contents of public_html” --author=”Tom<123@haha.com>"
```

关于日志消息,有时需要打开编辑器编写一条完整而且详细的日志消息,这 时就需要设置 GIT_EDITOR 环境变量。
在 tcsh shell 中,这样设置:
```
$ setenv GIT_EDITOR emacx
```
在 bash shell 中,这样设置:
```
$ export GIT_EDITOR=vim
```
关于作者信息,如果没有配置的话,在提交的时候也木有明确说明作者,就会遇到一些奇怪的警告。挺搞笑的,贴出来看一下(心里已经笑翻天):
You don't exist. Go away!
Your parents must have hated you!
You sysadmin must hate you!
下面介绍如何配置提交作者信息。

#### 配置提交者信息
Git 其实有一个作者的基本信息就够了,那就是名字和 email 地址。可以通过 git config 命令在配置文件中保存你的身份。

```
$ git config user.name “Tom”
$ git config user.email “123@haha.com”
```

还有一种方法就是直接设置 GIT_AUTHOR 和 GIT_AUTHOR_EMAIL 环境变量来配置。这些变量一旦设置就会覆盖所有的配置设置。

#### 查看提交
使用 `git log` 就会把版本库一系列的提交历史显示出来,每个条目都显示了提交作者的名字、eamil 地址、提交日期、变更的日志信息、提交的内部识别码。也可以使用 `git show 提交码`来查看特定提交的更加详细的信息还有一种查看方式是使用 `git show-branch –-more=N`,来查看当前 N 个分支的简洁的单行摘要。

#### 删除和重命名文件
从版本库里删除文件时,使用 `git rm 文件名`来删除文件,然后通过 git commit 命令来提交。重命名版本库里的文件时,使用`git mv 文件名1 文件名2`来重命名 文件,然后通过`git commit`来提交。当然这个操作也可以通过 `git rm` 和 `git add` 的组合来实现,举个例子:

```
$ mv file1.html file2.html 
$ git rm file1.html
$ git add file2.html
```

#### 创建版本库副本
可以通过 `git clone` 命令来创建一个完整的副本,这里这样举例:在主目录 中建立一个副本,并命名为 my_web
```
$ cd ~
$ git clone public_html my_web
```

一旦复制了一个版本库,就可以修改这个复制版本、做出的新提交、查看其
日志和历史等。这是一个有着完整历史的版本库。

#### 配置别名
这个是为了简化操作的,有的命令常用但是复杂,我们就可以为它设置一个 简单的 Git 别名。
```
$ git config --global alias.show-graph 'log --graph --abbrev-commit -- pretty=oneline'
```

在这个例子中,我们创建了一个 show-graph 别名,使用 `git show-graph` 这 个命令时,就如同已经输入了带所有选项的 `git log` 一样。

## 三、参考内容

1. [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd1836124857 8c67b8067c8c017b000)。这个哥写的东西不错,易懂
2. [Git 版本控制管理](https://book.douban.com/subject/26341974/),Jon Loeliger 写的,这里贴一句这哥们儿的名言吧 (我认为是名言)

> When Git needs to create a working directory, it says to the filesystem: “Hey! I have this big blob of data that is supposed to be placed at pathname path/to/directory/file.”

3. [Git 教程](http://www.runoob.com/git/git-tutorial.html),菜鸟教程,入门小助手