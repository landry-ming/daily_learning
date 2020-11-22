## Git_supplementary

****

****

##### 远程仓库的配置

1. `git remote -v`：查看有哪些远程仓库以及别名。
2. `git remote add origin https://github.com/landry-ming/huanshan.git`：给远程仓库配置别名。
3. `git push origin master`：将本地仓库推送到远程仓库origin的master分支。
4. `git clone https://github.com/landry-ming/huanshan.git `：将远程库克隆到本地，总共有三个效果，第一完整的把远程库下载到本地；第二创建origin远程仓库地址别名；第三初始化本地库。
5. 邀请团队成员：`repository > settings > manage access > collaborator`
6. `git pull origin master`：拉取操作相当于fetch+merge
   1. `git fetch [远程仓库别名(origin)] [远程分支名(master)]`。此时本地仓库的文件并没有改变，只是把远程仓库的文件获取了下来，可以切换到`git checkout origin master`去查看获取下来的文件内容
   2. `git merge [远程仓库别名(origin)] [远程分支名(master)]`
7. 解决冲突
   1. 如果不是基于Github远程库的最新版本修改，不能推送。需要先把最新版本拉取下来。
   2. 拉取下来后如果进入冲突状态，则按照"分支冲突解决"即可。

****

****







