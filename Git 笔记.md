# Git学习笔记总结
## 一、常用命令
1、配置电脑信息：
>git config --global user.name "你的名字"<br>
>git config --global user.email "你的邮箱地址"

	注： --global参数表示这台机器上所有的Git仓库都会使用这个配置

2、创建版本库:
>git init(将当前文件夹变成Git可以管理的仓库)

3、查看工作去状态：
>git status

4、 查看修改的内容:
>git diff 修改的文件名（包括后缀）

5、把文件添加到仓库：
>git add 文件名（包括后缀）

6、将文件提交到仓库：
>git commit -m "这次提交的说明"

7、查看提交的历史记录：
>git log
	
	注：可以加 --pretty=oneline 参数简化显示结果

8、版本回退：
>git reset --hard HEAD^

	注:(1)HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，···，上100个版本写成HEAD~100
	   (2)hard 后也可以是版本号，版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

9、查看历史命令：
>git reflog

10、丢弃工作区的修改：
>git checkout --要丢弃修改的文件名

11、撤销暂存区的修改：
>git reset HEAD 要撤销的文件名

12、从版本库中删除文件：
>git rm 文件名

13、查看当前分支：
>git branch

14、创建并切换分支：
>git checkout -b 分支名
	
	注：参数-b表示创建并切换，相当于以下两条命令：
		git branch 分支名
		git checkout 分支名
	   直接切换分支不需要参数 -b

15、合并分支：
>git merge 要合并的分支名
	
	注：git merge命令用于合并指定分支到当前分支。
	   合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

16、删除分支：
>git branch -d 要删除的分支名

17、查看分支合并图：
>git log --graph

18、“储藏”当前工作区：（常用于解决bug，创建bug分支）
>git stash

	注：git stash list 命令可以查看“储藏”信息

19、恢复“储藏”的工作区：
>git stash apply 或 git stash pop
	
	注：git stash apply 恢复后stash内容并不删除，你需要用git stash drop来删除
	    git stash pop 恢复的同时把stash内容也删了
	    可以储藏多个工作区，恢复指定工作区用：git stash apply stash@{0}，其中stash@{0}可以通过git stash list查看

20、删除没有合并过的分支：
>git branch -D 分支名

21、查看远程仓库信息：
>git remote -v
	
	注：参数 -v表示查看详细信息

22、克隆远程仓库：
>git clone 远程仓库地址
	
	注：ssh地址用: git clone git@github.com:AzazelX5/notes.git
	    https地址用: git clone https://github.com/AzazelX5/notes.git

23、从本地推送分支：
>git push -u origin 分支名

	注：加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

24、从远程仓库更新本地分支：
>git pull 
>>命令详解见[易百教程：git pull 命令](http://www.yiibai.com/git/git_pull.html)

25、标签：
>git tag 标签名
	
	注：命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
		git tag -a <tagname> -m "blablabla..."可以指定标签信息；
		git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
		命令git tag可以查看所有标签。
		命令git push origin <tagname>可以推送一个本地标签；
		命令git push origin --tags可以推送全部未推送过的本地标签；
		命令git tag -d <tagname>可以删除一个本地标签；
		命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

## 二、GitHub
### 1、设置SSH keys
#### （1）创建 SSH Key
* 在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：</br>
>          ssh-keygen -t rsa -C "你的邮箱地址"

	注：输入命令后一路回车，使用默认配置即可。如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人
#### (2) 在GitHub账户中添加共匙
* 登陆GitHub，找到设置（settings）页面，点击“SSH and GPG keys”选项，然后点“New SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容,最后点击Add SSH key。见下图：
![设置GitHub步骤](https://i.imgur.com/6JQdmTA.png)

### 2、将本地仓库关联到GitHub仓库
#### （1）创建GitHub仓库（见下图）
![](https://i.imgur.com/WzCAQDz.png)
![](https://i.imgur.com/twEXaqE.png)

	注：2图中步骤1为新建仓库名称，勾选步骤2后会自动创建README.md文件

#### （2）关联已有本地仓库
> git remote add origin git@github.com:**你的GitHub帐户名**/learngit.git
#### （3）将本地版本库内容推到GitHub仓库
> git push -u origin master

	注：把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数。
	
## 二、常见问题及解决方法
