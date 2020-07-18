## Git的使用

#### Git的概述

目前世界上最先进的分布式版本控制系统没有之一

#### 集中式VS分布式

集中式版本控制系统，版本库是放在中央服务器的，干活时先从中央服务器获得最新的版本，然后再开始干活，再把自己的活推给中央服务器。**集中式版本控制系统的弊端**在于必须要联网才能工作，如果在局域网内还好，如果发生在互联网上，可能会遇到网络延迟的问题，可能提交一个几十兆的文件就需要好几分钟。

分布式版本控制系统没有中央服务器，每个人电脑上都有一个完整的版本库，工作的时候无需联网，修改同一个文件时，只需将各自的修改推送给对方，就可以看到对方的修改了。在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

综上所述，分布式版本控制系统的安全性要高许多，某个电脑坏了，可以从其他人那里复制一份就可以了，集中式版本控制的话，一旦中央服务器出现问题，就无法进行正常工作了。

#### 创建版本库

版本库又称为仓库（**repository**），相当于一个目录，目录下的全部文件由Git管理。每个文件的修改、删除都能被Git跟踪，在任何时候都可以追踪、还原历史。

* 第一步创建一个空目录

![image-20200711092056468](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711092056468.png)

`pwd`命令用于显示当前的目录，上图表示我创建的仓库位于`/d/Personal/Desktop/learngit`

* 第二步`git init`命令将这个目录变成GIt可以管理的仓库：

![image-20200711093410130](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711093410130.png)

如图，Git就创建好仓库了，并显示为一个空的仓库（**empty Git responsitory**），发现当前目录下多了一个`.git`的目录，这个目录的作用是Git用于跟踪管理版本库的，**记住一定不要手动修改里面的文件，否则会破坏掉Git仓库！。**

#### 把文件添加到版本库

所有的版本控制只能跟踪文本文件的改动，例如TXT、网页、所有的代码程序等，Git也不例外。而图片、视频这些二进制文件，虽然也是有版本控制系统管理，但没法跟踪文件的变化，只能知道其文件大小的变化，到底改了什么，是没有办法知道的。

不幸的是Microsoft的World格式也是二进制格式的，因此版本控制系统无法跟踪Word文件的改动，如果真正使用版本控制系统，就得以纯文本方式进行文件编写。

文本都是有编码的，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，也被所有平台所支持。

* 第一步编写一个`readme.txt`文件，放到之前创建好`learngit`目录下

![image-20200711095900327](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711095900327.png)

* 第二步用命令`git add`告诉Git，把文件添加到仓库

![image-20200711100644648](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711100644648.png)

执行完上面的命令没有任何显示，就说明添加已经成功。

* 第三步用命令`git commit`告诉Git，把文件提交给仓库

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

嫌麻烦不想输入`-m "xxx"`行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

在原文本下添加了三个点，运行`git status`命令查看结果。

![image-20200711104423383](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711104423383.png)

`git status`命令可以时刻掌握仓库当前的状态，上图信息显示`readme.txt`被修改过了，但还没有准备提交的修改。

接下来我们查看下具体修改了什么内容，使用`git diff`命令看看。

![image-20200711104830198](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711104830198.png)

`git diff`顾名思义就是查看difference，可以从上面的命令输出看到，我在第二行末尾添加了`三个点`。

知道对`readme.txt`做了什么修改后，就可以放心提交到仓库了，提交修改和提交新文件是一样的两步，第一步是`git add`，第二步是`git commit`，在提交之前先看看`git status`。

![image-20200711105520558](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711105520558.png)

上图显示，将要被提交的修改包括`readme.txt`，下面就可以放心提交了，提交后再用`git status`查看仓库当前的状态。

![image-20200711105809273](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711105809273.png)

上图显示当前我们没有需要提交的修改，而且工作目录是干净的（working tree clean）。

`git log`命令显示从最近到最远的提交日志

![image-20200711150042475](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200711150042475.png)

`git log`命令显示从最近到最远的提交日志，可以看到有3次提交，最近的一次是`add another line`，上一次是`add a line`

Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本就比较容易数不过来，因此携程`HEAD~100`。

Git的版本回退速度非常快，因为Git内部有个指向当前版本的`HEAD`指针，`HEAD`可以定位到你想要的版本，如果你关闭了命令窗口，想回退版本时候，可以通过`git refolg`记录你的每一次命令。

![image-20200712232352629](C:\Users\68300110\AppData\Roaming\Typora\typora-user-images\image-20200712232352629.png)

