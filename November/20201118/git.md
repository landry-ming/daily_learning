## Git

#### 一.git 是什么

首先，git是一个版本控制工具。

版本控制(Revision control)是一种在开发过程中用于管理我们对文件，目录或者工程等内容的修改历史，方便我们查看或者更改历史记录，备份以便恢复以前的版本的软件工程技术。分为集中化的版本控制系统：SVN， 分布式的版本控制系统：Git。

集中化的版本控制：

1. 代码放在单一的服务器上，便于项目的管理。
2. 但是最显而易见的缺点是中央服务器的故障，会产生无法追溯历史版本等问题。
3. 在服务器当机时，也无法工作。
4. 若是中央服务器的磁盘故障，会有丢失数据的风险，最坏的情况彻底丢失整个项目的所有历史更改记录。
5. svn每次存的是版本之间的差异，需要的硬盘空间会相对少一些，可是回滚的速度较慢。

分布式的版本控制系统：

1. 客户端并不是只提取最新版本的文件快照，而是把代码仓库完整的镜像下来。
2. 分布式的版本控制系统在管理项目时，存放的并不是项目版本与版本之间的差异(集中式是)，他存放的是索引(所需磁盘空间很少，所以每个客户端都可以放下整个项目的历史记录)。
3. 断网的情况下也可以进行开发，版本控制是在本地进行的。使用github进行团队协作时，哪怕git挂了，每个客户端保存的也都是完整的项目(包含历史记录)。
4. git每次存的都是项目的完整快照，需要的磁盘空间会相对大一些(git 团队对代码做了极致的压缩，最终需要的空间比svn多不了很多)，可是git回滚的速度较快。

#### 二. git初始化的配置

1. `git config --global user.name ""`
2. `git config --global user.email `
3. 要检查已有的配置消息，可以使用`git config --list`
4. 



#### 三. git工作流原理

![Git 原理](https://user-gold-cdn.xitu.io/2020/5/24/172425d6ec60dbf1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

1. 名词
   1. **Workspace**：工作区，写代码的地方
   2. **index/stage**：暂存区，执行git add命令就把工作区内容提交到暂存区
   3. **Repository**：仓库区（或本地仓库），执行`git commit`命令就会把暂存区的内容提交到本地仓库
   4. **Remote**：远程仓库，执行`git push`命令就可以把本地代码推到远程分支

![img](https://user-gold-cdn.xitu.io/2020/5/24/172426bbb2c6c662?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![常用操作图示](https://user-gold-cdn.xitu.io/2020/5/24/172426bbb3a69a45?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)