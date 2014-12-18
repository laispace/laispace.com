title: 'SVN 常用命令'
date: 2014-05-16 10:54:29
---

        //svn up/cleanup/co/st/diff/ci/sw/merge/info
        
        // help 帮助
        svn help
        svn help ci
        
        // checkout 复制到本地
        svn checkout URL
        // 简写
        svn co URL
        
        // add 添加文件
        svn add FILE
        // rm 删除文件/目录
        svn rm FILE
        
        // commit 提交
        svn commit -m 'first commit'
        // 简写
        svn ci
        
        // update 更新到某个版本
        svn update -r m URL
        // 简写
        svn up
        
        // status 查看文件或目录状态
        // 不显示子目录状态
        svn status URL
        // 显示子目录状态
        svn status -v URL
        // 简写 
        svn st
        
        // log 查看当前工作副本
        svn log
        
        // delete 删除文件
        svn delete URL -m 'delete a file'
        
        // info 查看日志
        svn info URL
        
        // diff 对比差异
        // 被修改文件与基础版本对比
        svn diff 
        // 与最新版本对比
        svn diff -r head
        // 版本 m 与版本 n 对比
        svn diff -r m:n URL
        // 简写 
        svn di
        
        // merge 将版本 m 和版本 n 合并
        svn merge -r m:n URL
        
        // switch 切换到某一个版本
        svn switch URL
        
        // cleanup 回到一个稳定的状态
        svn cleanup

- SVN 小记
    
    创建分支:

    svn copy http://mysvn.laispace.com/myproj/trunk http://mysvn.laispace.com/myproj/branches/xiaolai -m '创建一个名为 xiaolai 的分支'

    拉取代码:

    svn checkout http://mysvn.laispace.com/myproj/branches/xiaolai

    在分支上编写代码后, svn commit 后, 切换到主干目录, 进行合并

    svn merge http://mysvn.laispace.com/myproj/branches/xiaolai

    合并完后, 查看已合并信息:

    svn mergeinfo http://mysvn.laispace.com/myproj/branches/xiaolai

    合并完后, 查看未合并信息:

    svn merginfo http://mysvn.laispace.com/myproj/branches/xiaolai --show-revs eligible


- SVN 分支 branch 与标记 tag 的区别
    
    分支用于在并行开发，这里的并行是指和trunk(主分支)的并行。
    而tag是用来做一个里程碑（milestone），不管是不是release，都是一个可用的版本。




![](http://lailife.u.qiniudn.com/xiaolai.jpg)