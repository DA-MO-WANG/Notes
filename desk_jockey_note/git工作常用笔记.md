```shell
#工作区、暂存区的概念
有两个概念圈：本地一个概念圈，远程一个概念圈
工作区概念==》本地所有的东西

```

​		1.工作区、暂存区的概念</br>
​				工作区就是我们看到的本地目录；暂存区是版本库里的一个概念。版本库物理上对应 .git目录。</br>
​				在初始化版本库时，git会自动创建一个本地分支-master</br>
​				具体的git命令和这些概念的联系：</br>
​						git add 是把文件放到暂存区；git commit是把暂存区的所有文件放到本地分支上</br>
​		2.SSH key的概念</br>
​				这是一个密码机制，本地一方和远程一方的校验。本地一方来生成一对key：公钥和私钥，公钥交给远方，来校验来自本地的互动。</br>
​		3.远程库的概念</br>
​			远程库和本地库本质都是一个git管理的仓库，本地库要和远程库互动，就要先建立关联</br>
​			git remote add 远程仓库名(默认origin) 远程仓库地址</br>
​		4.分支的概念</br>
​			分支类似平行宇宙，类似开时间线，类似快照。多开分支，是为了不影响别人拉下不能运行的代码；也方便自己随时提交，不会丢失本地开发进度</br>
​			分支的实质是指针技巧，git的很多操作实际只是单纯操作指针，实际物理变化基本没有</br>
​			所谓合并分支的实质就是让master指针指向最新的一次提交</br>
​			每新建的一个分支都是从当前分支派生出的一个子支线（先看清当前分支是啥）——》所有的操作是针对分支的，而且是把当前分支当作被动对象的，所以在操作之前先想清这个操作的作用对象是谁</br>

​		5.git命令的设计</br>
​			git +静态对象相关名词+动态的用参数表示+具体</br>
​		6.合并冲突的情况</br>
​			什么时候会发生合并冲突？当前分支和主分支的关系不再是包含和被包含的关系；而退化成交集关系：在开新分支后，master也有提交</br>
​			如何解决：就是在master里修改当前分支的变化，然后删掉其他分支</br>

​	
​					

​		7.提交冲突</br>
​				提交之前先拉取最新：git pull</br>
​				在本地修改冲突，然后重新提交到本地仓库</br>
​				提交到远程分支</br>

​		8.常见公司分支管理策略</br>
​			master分支用来发版用</br>
​			dev分支用来日常开发</br>
​			每个人的个人开发分支都是从dev派生出的</br>

​	    9.rebase ????






###### git在工作中的常见工作场景

​		1.简单提交一个文件</br>
​				git init : 把当前目录从一个普通目录转变为受git管理的仓库目录(一种逻辑概念)</br>
​				git add  具体文件名：把这个文件添加到本地git仓库，也就是受git 管理</br>
​				git commit (-m 具体提交描述)：通知git 把本地仓库内的文件提交到远程仓库</br>
​				
​		2.修改后，加入到git仓库，但没有立即提交，过了一段时间，想看看自己做了哪些变化</br>
​				git status :查看工作区状态，有没有被修改</br>
​				git  diff  具体文件名：查看这个文件对比上一次提交做了哪些变化</br>
​		3.后知后觉，觉得把文件改差了，要返回到开始改差的上一个刻重新开始</br>
​				git log : 从最近的一次提交到最远的一次提交，开始从上往下罗列</br>
​				git  reset  (--hard  指定版本号)：当前版本号用head代指，head^ 指head基础上上一个版本，几个^代表汪过去推几个版本--相对概念的版本号标记；也可以用绝对标记-commit ID</br>
​		4.在经历一大串修改后，突然发现改错了，但是还没推送到远端</br>
​			还没git add ---->git restore filename  或者 ideal. rollback</br>
​			git add了但还没git commit ---->git reset HEAD  filename </br>
​			git commit 但没git push ----> 回退版本</br>
​		5.工作区删除文件，版本库如何保持一致</br>
​			git rm filename(删除暂存区和工作区的内容)---然后git commit（改动提交到分支上）</br>
​		6.如何把本地推送到远程仓库？</br>
​			先建立本地仓库和远程仓库的关联：git remote add 远程仓库名  远程地址</br>
​			本地推送到远程：git push (-u 第一次提交时，建立本地和远程的分支关联)。远程仓库名(默认origin)  本地分支名(默认master)</br>
​			7.本地如何删除与远程库的绑定关系</br>
​			先查看远程库信息：git remote -v</br>
​			根据名字删除：git remote rm  origin(远程库名) </br>
​			8.本地如何克隆远程仓库相应分支版本</br>
​			多种协议可以选择，ssh最快</br>

​			9.分支管理！！！

​					前面很多工作区、版本库git操作前提都是建立在一定分支上的。先明确分支，默认是主分支</br>

​					创建并切换分支：git switch -c 新分支名(这只是本地创建)；创建远程分支：尾部加上 origin/ 对应分支名</br>
​					切换分支：git switch   分支名</br>
​					查看分支信息：git branch 分支名</br>
​					从当前分支合并到主分支：git  merge 当前分支名</br>
​					删除分支：git branch  -d 分支名(合并后过河拆桥删除)；还有合并之前放弃销毁：-d 变-D</br>
​			        把合并也当作一种提交来处理：在合并是加 --no-ff参数</br>

​			10.多人协作场景</br>
​						多个人拉取同一个dev分支，然后因为每个人不是同时提交，必然会出现第二个人提交时有冲突----》要求提交前，一定得先pull一下，让本地提交在dev上提交。这里就会涉及几个错误：</br>
​						no tracking ----->让本地和远程建立连接：git branch --set-upstream-to=origin/远程分支  本地分支</br>
​						解决冲突
​			11.打版场景</br>
​						用tag标签，代替commit ID</br>

​						怎么对一个分支打tag: 先切换到目标分支；git tag 版本号  对应的commitID(不写，默认是最新commitID)</br>						git tag 来查看现存的版本列表</br>
​						查看tag具体信息：git show 标签名</br>
​						给标签加说明： -a 版本号 -m  说明</br>
​		
​						推送到远程：</br>
​								git  push  origin 本地版本</br>
​								git push origin -tags :所有版本</br>
​						远程删除：</br>

​								git  tag -d 版本号</br>
​								git push origin :refs/tags/版本号	</br>													

​								

 



###### git 在工作中常见冲突如何解决

​		1.在dev上开发到一半，如何临时修改bug?</br>
​				笨办法是拉下来，修复，合并</br>
​				用git的命令工具：</br>
​						先保存工作到一半的现场：git stash (作用于当前分支)</br>
​						重新在一个分支上再开一个issue-101分支，修复提交</br>
​						如何恢复dev现场：先切换到dev分支，然后git stash pop (既恢复了又删除了临时快照)；git stash list 用来查看临时存储在什么地方</br>

​						若是dev分支上也存在：v
​								把修复的那个变化提交到dev上：(前提：先切换到dev分支-git branch确认一下)git cherry-pick bug修复的commit ID</br>

​        2.从master分支开一个dev分支，然后从dev分支开发，然后提交，然后合并到master	

​				在本地创建远程分支：先在本地创建本地分支，然后推送到远程：git push origin  本地分支
​				本地仓库相关的操作：
​						先添加到暂存区---
​						提交到本地当前分支上---建立当前分支和远程分支的连接：git branch --set-upstream-to=origin/远程分支  本地分支-----
​						git push (省略了  origin  远程分支：本地分支)
​						在远程平台操作：点branch---点new request，发起PR---等待有权限的人 merge 同意

​		3. 在2基础上，其他人先一步更新了dev，我如何推送

​				我直到推送失败，才知道远程已经相比我刚拉下时更新了
​				我先git pull 
​				git status  看下情况
​				手动修改文件里的冲突
​				把这个文件走一遍流程提交到本地分支里
​				git merge 合并，让master指向我最新提交
​				git push  推送到远端







```shell
#工作中常用命令
git log 文件a # 文件a所在目录下，a的git提交日志
git shortlog -sn #查看所有人的commit数
git branch -r #查看远程分支列表
```



```sql
#修改分支名称：本地、远程
git checkout old_branch_name #先切换到要修改的本地分支
git branch -m new_branch_name #改变本地分支名称
git push origin:old_reomote_branch_name new_branch_name #把本地更新过后的分支推到远程
git push origin -u new_branch_name #把更新新的上游分支
```

```sql
#开发新需求
拉取develope 分支代码 #基于develope 开新分支 
git checkout -b 新分支名 #基于当前分支创建新分支，并切换到新分支
git push -u origin 新分支名 #把当前分支推送到
```

