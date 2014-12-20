title: '设置 git/npm/bower/gem 镜像或代理的方法'
date: 2014-05-16 10:54:29
---

有时候我们在某些环境下(比如墙内或公司内网)可能不能正常使用 git/npm/bower/gem 等各种工具, 解决办法有:

- 切换镜像
- 使用代理
- 召唤五姑娘

不同工具设置的语法略有偏颇, 总结如下.

# 使用镜像

其中, npm/bower/gem 等如果可以通过切换镜像来解决的话, 则不用设置代理.

不知道有哪些镜像资源, 则 Google 之. 以下的 < registry url > 指的就是镜像的 url, 比如 http://registry.npm.taobao.org/

## npm 

设置全局使用指定的镜像:

    $ npm config set registry < registry url >

或者在安装时才指定:

    $ npm install --registry < registry url >

当然, 每次都要输入那么长串的 registry url 的话, 实在太麻烦, 可以使用 nrm 这个模块来切换镜像:
    
    // 全局安装
    $ npm install -g nrm
    // 查看有哪些镜像
    $ nrm ls
    // 对比各个镜像的访问速度
    $ nrm test
    // 使用淘宝的镜像
    $ nrm use taobao

## gem

    $ gem source -r <registry url>    

gem 除了使用镜像以外, 还可以直接到官网下载需要的包, 然后在本地安装,
比如我们要安装 sass, 先[到这里](https://rubygems.org/gems/sass) 把 sass 下载到本地, 然后在本地安装:

    // 注意这里的 sass.gem 是下载到本地的包名
    $ gem install --local sass.gem


# 使用代理

镜像不能用, 那就使用代理吧.

假定公司提供的代理为 http://proxy.mysite.com:8080

## 给命令行统一设置代理

- windows 

    $ set http_proxy=http://proxy.mysite.com:8080
    
    // 如果有要求用户名密码则输入: 
    $ set http_proxy_user=< username >
    $ set http_proxy_pass=< password >
    
若不想每次都手动设置, 则可以设置到系统的环境变量中
    
    右击计算机–>属性–>高级–>环境变量–>系统变量，设置系统变量

- mac 
    
    $ sudo networksetup -setwebproxy "Ethernet" http://proxy.mysite.com 8000

## git

设置:
    
    $ git config --global http.proxy http://proxy.mysite.com:8080

取消: 
    
    $ git config --global --unset http.proxy

## npm

设置:
    
    $ npm config set proxy=http://proxy.mysite.com:8080

取消: 

    $ npm config delete proxy

## bower

设置:
    
    修改 .bowerrc 文件(如无则新增):

        {
          "proxy": "http://proxy.mysite.com:8080",
          "https-proxy": "http://proxy.mysite.com:8080"
        }
    

取消: 
    
    删除 .bowerrc 里对应的配置即可

## gem

比如我们要安装 sass

设置:

安装时加上 --http-proxy 参数    

   $ gem install --http-proxy http://proxy.mysite.com:8080 sass

取消: 

安装时不加上 --http-proxy 参数    

    $ gem install --http-proxy http://proxy.tencent.com:8080 sass


# 召唤五姑娘

不能使用镜像, 又不能使用代理, 一般这个时候我们都会先哭一下, 然后选择离开这个行业, 去卖烧饼或者红薯什么的.

如果还对生活有希望的话, 那就使用我们勤劳的右手:
    
    在外网中, 下载好需要的东东, 拷贝到受限的机子中...


