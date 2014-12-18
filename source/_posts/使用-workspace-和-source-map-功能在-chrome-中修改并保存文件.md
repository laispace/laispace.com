title: 使用 workspace 和 source map 功能在 chrome 中修改并保存文件
categories:
  - Tools
date: 2014-09-01 15:33:32
tags:
  - workplace
  - sourcemap
 
---

1. 创建测试文件

   创建 test/test.html，引入 test/test.css 

2. 打开本地服务器
    
    这时候审查 test.html 的页面元素修改其 css 属性并不会生效。

3. 建立本地和服务器的文件映射

    1. 打开 chrome dev tools
    2. 设置 Workspace，添加文件夹 test/
    3. 双击这个新增的文件夹，增加映射
        
        URL prefix 填写浏览器地址栏对应的服务器根目录

        Folder path 填写本地文件夹的路径

4. 审查元素，修改 test.html 中的元素的 css 属性

    用编辑器打开 test.css 就会发现修改保存到了文件中了。


5. 使用 Sass 更进一步
    
    上述方法能直接将我们在浏览器中对 css 的修改保存到本地文件中，

    但我们可能不是直接编写 css 文件，而是使用了 Sass/Less 等预编译工具，这时候 source map 功能就派上用场了。

6. 配置 source map 让 chrome 支持 Sass 

    小赖使用的是 Webstorm 的 Sass 插件，其能监听 scss 文件的改变，自动生成新的 css 文件。

    但默认一个 scss 文件只会生成对应的 css 文件，我们按以上的方法在浏览器中只能修改 css 文件，而不能修改源 scss 文件。

    解决方法是：

        1. 修改 webstorm 的 sass 插件配置
            
                --no-cache --update $FileName$:$FileNameWithoutExtension$.css
            
            修改为：

                --sourcemap --no-cache --update $FileName$:$FileNameWithoutExtension$.css
            
            这时候 test.scss 在自动编译时不仅会生成 test.css 文件，还会生成 test.css.map 文件

        2. 开启 chrome 的 source map 功能

                打开 dev tools 后，勾选 General->Enable CSS source maps 

    这时候，我们审查 test.html 中的元素，则会看到对应的 test.scss 文件
    
    但在浏览器中修改 css 属性时，实际上修改的是 test.scss 编译生成的 test.css 文件。

    我们希望直接修改的是 test.scss 文件，这时候点击 dev tools 中的 Source，找到对应的 test.scss 文件修改后 cmd + s 保存即可。

7. 第6点其实有点鸡肋

    目前 source map 能让我们在浏览器中修改 css 属性后保存到对应的 css 文件中。

    而要修改 Sass 文件只能借 Workspace 功能在 Source 面板中修改后保存，但这时候修改的 Sass 文件并不会自动编译为对应的 css 文件。

    在浏览器中修改 Sass 文件保存到本地后，浏览器中的 css 并没有即时改变。






    
