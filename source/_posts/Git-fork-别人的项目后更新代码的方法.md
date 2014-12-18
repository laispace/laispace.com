title: Git fork 别人的项目后更新代码的方法
categories:
  - Tools
date: 2014-08-02 19:19:40
tags:
  - git			
---

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

