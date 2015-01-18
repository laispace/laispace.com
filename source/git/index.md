title: 'GIT 常用命令'
date: 2014-05-16 10:54:29
---
HEAD指向的版本就是当前版本


- log 记录

        # 查 看提交历史
        $ git log
        
        # log --graph 查看分支合并图
        $ git log --graph
        
- reflog 命令历史

        # 记录命令的操作历史
        $ git reflog        
        
- reset 回退

        # 回退到上一个版本
        $ git reset --hard HEAD^
        
        # 回退到上两个版本
        $ git reset --hard HEAD^^
        
        # 回退到上48个版本
        $ git reset --hard HEAD~48

        # 回退到以 id 为 3628164 的版本
        $ git reset --hard 3628164  
        
- remote 远程

        # remote add 添加远程仓库             
        $ git remote add origin git@server-name:username/repoName.git

        # remote -v 查看远程仓库
        $ git remote -v 
        
- push 推送到远程
        
        # 将本地的主分支推送到远程
        $ git push -u origin master
        

- pull 从远程拉取
    
        $ git pull

- clone 从远程克隆

        $ git clone git@server-name:username/repoName.git
    
    
- branch 分支     

        # 查看当前分支
        $ git branch
        
        # branch filename 创建分支
        $ git branch testBranch
        
        # branch -d 删除分支        
        # 合并成功后，删除 testBranch 分支
        $ git branch -d testBranch 
        
        # 若合并失败，则手动解决冲突后，执行
        $ git add conflictedFile.md 
        $ git commit -m "conflict fixed"    
        
        # - branch --set-upstream 关联本地与远程分支
        # 建立本地分支与远程分支的关联，名字最好一致！
        $ git branch --set-upstream branch-name origin/branch-name； 
                
- checkout 切换

        # checkout -b 创建并切换分支
        # 创建一个名为 testBranch 的分支，-b 表示创建并切换
        $ git checkout -b testBranch
        # -b 表示创建并切换，相当于：
        # git branch testBranch
        # git checkout testBranch

        # checkout -- 撤销
        # 把 README.md 在工作区的修改全部撤销(回到最近一次git commit或git add时的状态)
        $ git checkout -- README.md 
        
                
        # checkout filename 切换分支        
        # 切换回主分支
        $ git checkout master
        
- merge 合并分支
        
        # 将 testBranch 分支的内容合并到主分支
        $ git merge testBranch

- tag 标签

        # 创建标签
        $ git tag v2.0

        # 查看所有标签
        $ git tag
        
        # 查看特定标签信息
        $ git show v2.0
        
        # 删除标签
        $ git tag -d v2.0
        
        # 创建的标签都只存储在本地
        # 手动推送标签到远程
        $ git push origin v2.0
        
        # 手动推送所有标签到远程
        $ git push origin --tags
        
- 自定义配置

        # 配置别名，如 st 表示 status，co 表示 checkout，ci 表示commit，br 表示 branch
        $ git config --global alias.st status   
        $ git config --global alias.co checkout
        $ git config --global alias.ci commit
        $ git config --global alias.br branch
    
        

- 忽略文件被git追踪：

项目根目录下创建.gitignore文件后将需要忽略的文件写入即可。

除自己新建手写这个文件外，可在[这里](https://github.com/github/gitignore)看到配置好的 .gitignore       



- fork 别人的项目后更新代码的方法

1. 举个例子，需要 fork 这个项目 https://github.com/tarobjtu/WebFundamentals.git

2. 点击 fork, 就会复制一份代码到自己的 repo https://github.com/laispace/WebFundamentals.git

3. 本地 clone 自己 repo 中的这个项目

    $ git clone https://github.com/laispace/WebFundamentals.git

4. 添加自己的远程仓库
    
    $ cd WebFundamentals
    $ git remote add laispace https://github.com/laispace/WebFundamentals.git

5. 修改代码后进行 push

    $ git add --all
    $ git commit -m 'edit some files'
    $ git push

这时候，如果源仓库 tarobjtu 的项目代码进行了更新，而我们自己 fork 下来的代码想要合并这些更新怎么做呢？

6. 添加源项目的远程仓库

    $ git remote add tarobjtu https://github.com/tarobjtu/WebFundamentals.git
    // 这时候可以看到有两个源了
    $ git remote  
    // laispace
    // tarobjtu

7. 拉取源仓库的代码到本地
    
    $ git fetch tarobjtu

8. 合并源仓库的 master 分支代码到本地

    $ git merge tarobjtu/master

9. 提交代码到我们自己的仓库

    $ git add --all
    $ git commit -m '合并源仓库代码'
    $ git push


# 删除提交记录的办法
git reset --hard HEAD~2 # 取消之前的两次提交
git push origin HEAD --force # 强制提交到，删除之前的数据

### 参考资料

1. [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)                  
        
        
        
