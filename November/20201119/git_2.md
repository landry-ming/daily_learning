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
   1. `git config --system`：对系统中所有的用户都普遍适用的配置，配置文件在/etc/gitconfig。
   2. `git config --global`：使用于该用户，配置文件在~/.gitconfig。
   3. `git config `：配置只对当前项目有效，在当前项目的配置文件中.git/config.

#### 三. git工作流原理

![Git 原理](https://user-gold-cdn.xitu.io/2020/5/24/172425d6ec60dbf1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

1. 名词
   1. **Workspace**：工作区，写代码的地方（沙箱环境）
   2. **index/stage**：暂存区，执行git add命令就把工作区内容提交到暂存区
   3. **Repository**：仓库区（或本地仓库），执行`git commit`命令就会把暂存区的内容提交到本地仓库
   4. **Remote**：远程仓库，执行`git push`命令就可以把本地代码推到远程分支

![img](https://user-gold-cdn.xitu.io/2020/5/24/172426bbb2c6c662?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![常用操作图示](https://user-gold-cdn.xitu.io/2020/5/24/172426bbb3a69a45?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### git 底层命令

1. .git目录

   1. hooks: 目录>包含客户端或者服务端的钩子脚本。
   2. info：包含全局性的排除文件
   3. logs：保存日志消息
   4. **objects：**目录>存储所有数据内容
   5. **refs：**目录>存储指向数据(分支)提交对象的指针
   6. config：文件包含项目特有的配置信息
   7. description：用来显示对仓库的描述信息
   8. **HEAD：**文件>指示目前被检出的分支
   9. **index：**文件>保存暂存区信息

2. 基础的linux命令

   1. clear：清除屏幕
   2. echo “test content” (> 文件名)：向控制台或者文件中输出信息
   3. find 目录名：将对应目录下的子孙文件&子孙目录平铺在控制台
   4. find 目录名 -type f ：将对应目录下的文件平铺在控制台
   5. rm 文件名：删除
   6. mv 文件名 新文件名：重命名
   7. vim 文件：点i进入文件的插入模式，可编辑。esc+：wq保存退出；esc+:q：强制退出。esc+ set nu：设置行号。

3. git对象

   1. git的核心是一个简单的键值对数据库，你可以向该数据插入任意类型的内容，他会返回一个键值，通过该键值可以在任意时刻再次检索该内容。

   2. key是val对应的hash。键值对在git的内部是一个blob类型。

   3. git对象存在的问题：

      1. git对象并非一次项目或者一次版本的快照，仅仅作为一次文件的修改增加或者删除等操作存储为git对象，被保存在git数据库中，返回标识的hash值。
      2. 在git对象中，文件名并没有被保存，只能通过哈希值访问，记住哈希值是不现实的。
      3. git对象的操作只是针对本地的数据库进行操作，并不涉及暂存区(暂存区无内容)。

   4. ```linux
      echo "test contest":向控制台输出内容test content
      echo "test contest" > file文件名：新建文件夹并写入内容test content
      这两个是linux命令，可以向控制台或者文件写入内容。
      
      echo "test content" | git hash-object --stbin：返回该内容的哈希值
      git会返回一个哈希值用来标识这句话，但是并不会写到数据库中。其中`--stbin`参数是指示该命令从标准
      输入中读取内容，若不指定该选项，则须在命令的末尾给出存储文件的路径。
      
      echo "test content" | git hash-object -w --stbin
      这句命令返回一个hash值用来标识这句话 并写到数据库中.-w参数命令存储数据对象，不指定此选项，此命令
      仅返回对应的值。
      该文件会被写到数据库中(./git/object)。可以通过`find ./.git/object -type f`找object文件下的文件名。但是因为里面的内容被压缩。可以通过`git cat-file -t 文件对应哈希值`来查看文件。`git cat-file -t 文件对应的哈希值`来查看git对象的类型>blob类型。
      
      git hash-object filename：返回该文件的哈希值  > git hash-object ./test.txt
      git hash-object -w filename：存储该文件并返回该文件的哈希值 > git hash-object -w ./test.txt
      ```

4. 树对象

   1. 树对象，**他能解决文件名的保存问题。允许我们将多个文件组织在一起**。Git以一种类似于unix文件系统的方式存储内容。所有内容以树对象和数据对象 (git对象) 的形式存储，其中树对象对应unix的目录，数据对象 (git对象)则大致上对应文件内容，一个树对象包含一条或多条记录(每个记录含有一个指向git对象或者子树对象的hash指针，以及对应的模式，类型和文件名信息)。一个树对象也可以包含另一个树对象。

   2. 构建树对象

      1. git update-index --add --cacheinfo 100644  哈希值  文件名 

         1. ```
            文件模式为100644 表明这是一个普通文件
            文件模式为100755 表明这是一个可执行文件
            文件模式为120000 表明这是一个符号连接
            --add 因为此前该文件并没有在暂存区中 首次要加add
            --cacheinfo 因为要添加的文件在git数据库中,没有位于当前目录下
            这个会将git对象保存文件名并提交到暂存区，版本数据库并没有更新什么。
            ```

      2.  git write-tree

         1. ```
            这会生成一个树对象，相当于给暂存区拍张照片放到版本库中，生成树对象。此时暂存区里只有update-index的git对象。并返回树对象的哈希值。
            此时可以通过git cat-file -p 哈希值；查看树对象内容或者git cat-file -t 哈希值 查看树对象类型。内容里有git对象的快照。类似于：
            100644 blob def1e6647e0b01988cbef5ae5b3b04cb37122abc    test.txt
            如果用find ./.git/objects -type f来查看会发现数据库中有一个树对象用来存放暂存区快照，还有git对象用来存放数据。
            
            git对象代表文件的一次次版本，数对象代表项目的一次次版本。
            
            write-tree通过生成树对象将暂存区里的内容写到版本库中，但是不会清空暂存区。
            
            新增文件
            echo "this is new file" > new.txt
            git hash-object -w new.txt
            vim test.txt
            git hash-object -w test.txt
            git update-index --cacheinfo 100644 test.txt哈希值
            git update-index --add new.txt(相当于前面的两步合为一步，第一步是将new.txt生成
            git对象，第二步是通过update-index将git对象上传到暂存区)
            ```

      3. 存在的问题：

         1. 不知道hash值对应哪一个版本，不知道这些版本的基础信息(谁提交的，提交日期，上一个版本等)

5. 提交树对象

   1. ```
      - commit-tree创建一个提交对象，为此需要指定一个树对象的hash值,以及该提交的父提交对象  
      - echo "second commit" | git commit-tree  019fb2c522b604cd94929085bbac93d60e2f2063 -p  d248eb19a125c
      - 真正代表一个项目的是一个提交对象（数据和基本信息）这是一个链式的！！ 
      
      因此git每次版本存的不是版本与版本之间的差异，而是版本的快照。流程是先将文件通过hash-object整理成git对象，再将git对象通过update-index上传到暂存区，write-tree将暂存区的git对象快照生成树对象，git-commit将树对象和提交信息等封装在一起。
      ```

#### git高层命令

##### 1. `git add 文件名`

1. git add 文件名不仅仅是把文件放在暂存区，而是先把每个文件做成独立的.git对象放到版本库(./.git/object)，再把git对象放在暂存区。相当于底层操作为1. `git hash-object -w 文件名` 2. `git update-index --add --cacheinfo 100644 哈希值 文件名` 。可以通过`git ls-files -s`查看缓存区。
2. 如果`git add 目录`：就是递归跟踪该目录下的所有文件。
3. 我们一般直接在工作路径下直接`git add ./`

##### 2.`git commit -m "注释"`

1. git commit会将暂存区的git对象快照生成树对象，再将树对象加上注释信息提交形成commit对象。即`git add`在版本库中会生成git对象，`git commit`会在版本库中生成树对象以及commit对象。
2. `git commit`执行的底层操作有1. `git write-tree` 2. `echo "commit" | git commit-tree 哈希值 -p 父树哈希值`。
3. 可以直接`git commit`进入vim编辑器，写入较长的注释。
4. `git commit -a -m 注释`：git就会自动把所有已经跟踪过的文件暂存在一起一起提交，从而跳过git add步骤。
5. 工作目录下面的所有文件都不外乎两种状态：已跟踪或未跟踪 (tracked untracked)。已跟踪的文件是指本来就被纳入版本控制管理的文件，在上次快照中由他们的记录，工作一段时间后，他们的状态可能是已提交，已修改或者已暂存。初次克隆某个仓库时，工作目录中的所有文件都属于已跟踪文件，且状态为已提交。在编辑过某些文件后，git将这些文件标为已修改，我们逐步把这些修改文件放到暂存区域，直到最后一次性提交所有这些暂存起来的文件。

##### 3. `git status`

1. 检查文件的状态 
2. untracked（未跟踪，可以通过git add 文件名跟踪）

	2. changes to be committed : new file 未跟踪的文件提交到暂存区。
 3. nothing to commit：说明文件已经提交。
 4. changes not staged for commit : modified：文件被修改，但是没有提交到暂存区。
 5. 注意，假设有一个文件名test.txt,你已经通过git add放入到暂存区，但是也还没有提交。此时你做了进一步修改，这时使用git status，你会发现test.txt有两种状态，一种是还未提交(位于暂存区)，一种是修改。这时候如果你提交，是将暂存区的版本提交。这是需要执行git add test.txt覆盖掉暂存区的文件，保留最新版本。

##### 4. `git diff`

1. `git diff`：当前做的哪些更新还没有被暂存。
2. `git diff --cached`， `git diff --staged`：有哪些更新已经被暂存但还未被提交。

##### 5. `删除操作`

1. 删除操作可以直接在工作环境中进行 `rm 文件名`， 然后将文件夹放入暂存区`git add ./`，这时提交文件将相当于对暂存区进行一次快照，此时不包含删除文件了而已。在git眼里删除操作本质上时一种修改操作。
2. `git rm 文件名`：这个操作会将文件删除后自动添加到暂存区。相当于将`rm 文件名`和`git add `合二为一。

##### 6. `文件改名`

1. 文件的改名也时直接在工作环境中进行，可以直接使用`mv 原文件名 新文件名`。此时在git status中，相当于删除了原文件名， 新增了新文件。此时统一git add就可以了，然后再commit提交就可以。
2. `git mv 原文件名 新文件名`：同git rm

##### 7. 查看历史记录：`git log`

1. 上下键查看， q退出
2. `git log --pretty=oneline`：让哈希值和版本的注释显示在同一行。
3. `git log --oneline`

****

****

#### git 分支

1. 分支含义：
   1. 每一个功能都可以开一个分支，不影响主线的分支
   2. 分支就是一个活动的指针，就在提交对象的前面指向最新提交
   3. 分支可以在`.git/refs`文件夹下查看，打开分支里面的内容是一串哈希值，指向最新提交的commit对象。
   4. master默认是主分支
2. 常见操作：
   1. `git branch test`：创建新的分支test
   2. `git branch`：显示分支列表
   3. `git checkout test`：将分支转到test上
      1. 切换分支的时候一定要提交完的时候再切否则会出现问题
      2. 在切换分支时，一定要注意你工作目录里的文件也会被改变。如果切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交的样子。
      3. 在切换分支时，会动三个地方，第一个地方指针指向当前分支。第二个地方暂存区。第三个地方工作目录。所以每次切换分支前，当前分支一定要提交干净，可能会污染分支或者主分支。如果提交到暂存区时，git会不允许切换分支，若是untracked，会被带到切换分支。
   4. `git checkout -b test`：创建新的分支test并切换到test
   5. `git branch -d test` 删除分支 不能自己删自己 
      1. 当分支没有被merge，需要将d换成D强制删除。
   6. `git log --oneline --decorate --graph --all`：查看完整的分支图
      1. 可以为每个命令配别名：`git config --global alias.lol "log --oneline --decorate --graph --all"`这样就会将上述命令配置称`git lol`
      2. `git config --global alias.lol log --oneline --decorate --graph --all`：命名比较长的时候要加双引号。
   7. `git branch -v` 查看分支的最后一个提交
   8. `git branch name hash`：新建一个分支并将分支指向对应的提交对象‘
   9. `git merge 分支名`：合并分支。
      1. `git merge --no-ff`：禁用快速合并。
      2. 分支里面存的是该分支所最后一次提交commit对象的hash值。

****

****

#### git存储

1. 如果你在一个分支上做了未完成的事情时需要切换到另外的分支中，但是不太想提交，这时候不提交有没法切换分支。
2. `git stash`命令会将未完成的修改保存在栈上。此时利用`git stash list`会显示存储的工作栈。
3. `git stash apply`：重新应用这些修改  +  `git stash drop` + 加上将要移除的储藏的名字来移除它。
4. `git stash pop`：应用储藏然后立即从栈上扔掉它。

****

****

#### 撤销&重置

1. 工作区的修改：`git checkout --文件名`。适用于在工作区改动后想要抛弃掉这些修改。

2. 暂存区的修改：`git restore --staged 文件名` 或者`git reset HEAD 文件名`将文件撤销到工作区。

3. 版本库的修改：

   1. 注释写错了：`git commit --amend`将暂存区的文件修改注释重新提交。如果想撤销掉这次提交，`git reset --soft HEAD^`。
   2. 内容写错了，重新写一份文件提交到暂存区覆盖到暂存区再提交就可。如果内容写错了，修改完内容后再重新提交时也可以用`git commit --amend`去修改。假设执行完`git commit -m "error"`发现错了，可以先修改文件，再`git add` 最后`git commit --amend`。

4. **Reset 命令**

   reset 移动 HEAD 指针，HEAD 可以是 hash值，~代表上一个指针

    ```
   git reset --soft HEAD~
   还可以用：
   git reset --soft 哈希值回到被改之前的文件
    ```

   与 checkout 不同，reset 移动整个分支，而 checkout 只是移动 HEAD 指针，它本质上是撤销了上一次的 git commit 命令。当再次运行 git commit 时， Git 会创建一个新的提交，并移动 HEAD 所指向的分支来使其指向该提交。

   当使用 reset 时候，其实就是把该分支移动回原来的位置，而**不会改变暂存区和工作目录**，而这个就是 git commit --amend 的原理。

   ![img](http://note.youdao.com/yws/public/resource/aa402b3b172d3876c5af053d2c790e54/xmlnote/66557F07ADD34C17B1BBDE5626BD191A/19347)

   

   与 soft 不同的是，--mixed 参数会更改暂存区，不会改工作目录。还原为之前的版本

    ```
   git reset [--mixed] HEAD~  # 默认为 --mixed 可不填写 
    ```

   只有 --mixed 模式可以跟文件名或路径，如果加了路径，则不会对HEAD进行跳转，只会撤销暂存区文件。因为 HEAD 指向一个分支，而分支代表一个提交对象（包括树对象，多个Git 对象）所以如果加了路径的话，不可能移动 HEAD。所以只更改暂存区。

   

   --hard 既会更改暂存区也会更改工作区，还原为之前的版本，(checkout也会修改三个地方)

                   ```
   git reset --hard HEAD~
   git reset --hard hash
                   ```

   注意： --hard 标记是 reset 命令唯一危险的用法，它也是 Git 会真正销毁数据的仅有的几个操作之一。 --hard 会强制覆盖工作目录和暂存区的文件。如果文件被提交，我们还可以从数据库中找到 Git 对象进行还原，否则如果未提交的话，该文件彻底消失。

   --hard 与 checkout 区别：

   ​	checkout commit’hash       reset --hard commit'哈希值

   1. checkout 只动 HEAD， --hard 动 HEAD 而且带着分支一起走

   2. checkout 对工作目录是安全的， --hard 强制覆盖工作目录

#### 数据恢复

1. 可以在 ./git/logs/HEAD 查看所有变更 HEAD 的记录，如果用 --hard 硬重置到其他分支，造成文件丢失的情况，可以改变 HEAD 硬重置回之前的分支来恢复文件。
2. `git reflog  # 查看所有变更 HEAD 的记录  
3.   git branch recover-branch branch_hash。重新创建一个分支，指向自己之前的分支，就可以恢复文件了。这种方法更为常用。      

#### Tag—标签

git 可以给历史中的某一个提交打上标签，一般用标签来标注版本号

1. **查看标签**

```
git tag  # 列出所有tag标签
git tag -l 1.8.5*  # *为通配符，列出1.8.5xxxxxx之类的所有tag
```

2. **创建标签**

```
git tag v1.4  # 标注最后一次提交对象
git tag v1.4 commithash  # 标注指定提交对象
```

3. **查看特定标签**

```
git show v1.0 
```

4. **删除标签**

```
git show -d v1.0
```

5. **检出标签**

```
git checkout v1.0
```

在检出标签模式下，会处于头部分离状态，可以查看当前分支的状态，但是最好不要commit，如果此时进行了提交，标签不会发生变化，新提交也不会属于任何分支，并且无法访问。除非访问确切的Hash。

因此，如果想要修改，最后使用 git branch -b branch_name 来创建分支进行修改



