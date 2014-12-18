title: Goagent使用心得
tags:
  - goagent
id: 401
categories:
  - Tools
date: 2013-06-01 12:02:58
---

Goagent实在是太棒了，可畅游被Qiang了的网站。初次安装可详见 [https://code.google.com/p/goagent/wiki/InstallGuide](https://code.google.com/p/goagent/wiki/InstallGuide)
<div></div>
<div>小赖使用总结：</div>
<div>     1.goagent突然上不去了？</div>
<div>        可能是国内cn的代理IP被封了，这时候可打开 local/proxy.ini **修改profile = google_cn 为 profile = google_hk**</div>
<div></div>
<div>     2.翻Qiang点击切换按钮麻烦？</div>
<div>**        打开 SwitchySharp -&gt;选项-&gt;切换规则，点击【新建规则】**，编写如下：注意 *://*.twitter.com/* 里的*是通配符，指可匹配任意字符</div>
<div></div>
<div>
<div>     3.手动切换代理很麻烦？</div>
<div>        按2的设置后，点击 **SwitchySharp-&gt;自动切换模式**，那么每次浏览时，goagent就会按建立好的切换规则来选择是否使用代理来翻Qiang了</div>
<div></div>
</div>
<div>     4.打开goagent.exe麻烦？</div>
<div>        找到**goage.exe右键创建快捷方式，把它放到 开始菜单-所有程序-启动 的文件夹里**（win7下即C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup）</div>
<div></div>
<div>     5.还想开机自启动？</div>
<div>        按3和4设置后，每次只要一开机，goagent就启动了，每次只要一翻Qiang,goagent就自动代理了。</div>
<div>        打开local/proxy.ini 找到**visible = 1改为visible = 0可将goagent隐藏在任务栏**哦</div>
<div></div>
<div>     6.看完一部片（别邪恶啊）就上不去了？</div>
<div>         Goagent一个APPID有1G/天的流量使用，刷刷twitter玩玩facebook就够了，可要是在youtube上看视频耗掉了流量，1G耗完就会显示 “[goagent 服务器未发送任何数据，因此无法加载该网页](https://www.google.com/search?q=goagent++%E5%A4%B1%E8%B4%A5&amp;oq=goagent++%E5%A4%B1%E8%B4%A5&amp;aqs=chrome.0.57j62.6423j0&amp;sourceid=chrome&amp;ie=UTF-8#newwindow=1&amp;safe=strict&amp;sclient=psy-ab&amp;q=goagent++%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9C%AA%E5%8F%91%E9%80%81%E4%BB%BB%E4%BD%95%E6%95%B0%E6%8D%AE%EF%BC%8C%E5%9B%A0%E6%AD%A4%E6%97%A0%E6%B3%95%E5%8A%A0%E8%BD%BD%E8%AF%A5%E7%BD%91%E9%A1%B5&amp;oq=goagent++%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%9C%AA%E5%8F%91%E9%80%81%E4%BB%BB%E4%BD%95%E6%95%B0%E6%8D%AE%EF%BC%8C%E5%9B%A0%E6%AD%A4%E6%97%A0%E6%B3%95%E5%8A%A0%E8%BD%BD%E8%AF%A5%E7%BD%91%E9%A1%B5&amp;gs_l=serp.3...2718.2718.1.3071.1.1.0.0.0.0.114.114.0j1.1.0...0.0.0..1c.1.15.psy-ab.EcsF0oX25Z8&amp;pbx=1&amp;bav=on.2,or.r_cp.r_qf.&amp;bvm=bv.47244034,d.dGI&amp;fp=3d441b6fcb5f99ae&amp;biw=1366&amp;bih=677 "goagent 服务器未发送任何数据，因此无法加载该网页 - Google Search")“，这并不是像1所说的代理IP被封了或者goagent自己抽风了，而是当前APPID的1G流量已经用完。打开https://appengine.google.com/dashboard?&amp;app_id=s~**yourAppid** 就可以看到Outgoing Bandwidth为100%啦！</div>
<div></div>
<div><span style="font-family: Arial, sans-serif;">     7.出现了6所说的情况流量不够用？</span></div>
<div><span style="font-family: Arial, sans-serif;">       可回到</span>[https://appengine.google.com/](https://appengine.google.com/)，点击【Create Application】，输入Application Identifier 如myappid-2（要check Availability），输入Application Title（可任填），然后再次点击【Create Application】。然后**打开 local/proxy.ini 编辑 appid = myappid-1 | myappid-2 （添加myappid-2，<strong>用 | 隔开**）</strong>，再**打开servr/uploader.bat 输入 myappid-2**，就可以将这个新的配置上传了，打开https://appengine.google.com会发现myappid-2在Running,代表新增的myappid-2已经激活，重启goagent.exe，打开浏览器，又可以继续翻Qiang啦！</div>
<div></div>
<div>     以此类推，可最多申请十个APPID，共10G的流量。</div>
<div></div>