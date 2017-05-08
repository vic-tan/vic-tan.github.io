---
layout: post
title: Mac下使用Aria23下载教程——迅雷和百度盘终极解决方案
date: 2017-01-15
categories: blog
tags: [Mac]
description: Mac下使用Aria2下载教程——迅雷和百度盘终极解决方案

---

大家在很多时候都希望让百度云盘和迅雷下载速度过百，但真实只有开通了会员才可能实现，其实在普通用户情况下下载，百度和迅雷是做了限制的，不允许我们过速度的下载，于是在网上找了一下，无意中发现下载神器aria2，后来发现mac端也可以直接安装aria2，由于aria2支持多线程下载，所以迅雷离线和百度盘基本能实现满速下载（实测可以达到 >10MB/s），且cpu占用率非常低。终于摆脱了蛋疼的mac端迅雷和百度云同步盘，特来发个帖总结一下最近的心得——————ps: 本教程适合爱折腾的童鞋，对terminal有抵触情绪的童鞋可以不用往下看了—



### 第一步：安装Aria2
下载最新的mac版aria2c（目前是1.19.3）

[https://github.com/tatsuhiro-t/aria2/releases/download/release-1.19.3/aria2-1.19.3-osx-darwin.dmg](https://github.com/tatsuhiro-t/aria2/releases/download/release-1.19.3/aria2-1.19.3-osx-darwin.dmg)

![http://bbs.feng.com/static/image/filetype/dmg.gif](http://bbs.feng.com/static/image/filetype/dmg.gif) [aria2-1.19.3-osx-darwin.dmg ](https://pan.baidu.com/s/1qXIdsPe)

下载好之后安装，安装应该没什么问题
安装目录在 /usr/local/aria2 下


### 第二步： 下载Aria2所需文件

首先下载附件中的配置文件aria2.conf（也可以自己新建一个配置文件），运行Aria2所有的选项都可以在配置文件中设置
想具体了解配置文件可以参考以下网站：[http://aria2c.com/usage.html ](http://aria2c.com/usage.html )或[http://aria2.sourceforge.net/manual/en/html/aria2c.html](http://aria2.sourceforge.net/manual/en/html/aria2c.html)

下载配置文件：![http://bbs.feng.com/static/image/filetype/zip.gif](http://bbs.feng.com/static/image/filetype/zip.gif)  [aria2.conf.zip ](https://pan.baidu.com/s/1o8Q5jNs)解压，用文本编辑打开aria2c.conf, 第二行是设置下载路径，


```python
dir=/Users/XXX/Downloads
```

把XXX改成你的用户名
接下来打开terminal，输入：


```python
mkdir ~/.aria2
```

用户根目录（/Users/XXX， XXX是你的用户名）下会生成一个.aria2的文件夹(隐藏文件夹)，将解压出来的配置文件aria2.conf 拖入这个文件夹中（这一步是为了方便每次启动aria2c的时候不用每次手动输入配置文件的位置）
接着下载：![http://bbs.feng.com/static/image/filetype/zip.gif](http://bbs.feng.com/static/image/filetype/zip.gif) [aria2c.zip](https://pan.baidu.com/s/1skKdicH)   解压后将aria2c文件夹整个拖入 /Applications 目录下

### 第三步： 运行Aria2

依旧在终端里，输入

```python
aria2c
```

如果第二步中的文件放置的位置没问题那么aria2c应该已经启动了

查看aria2c 是否启动：terminal中输入：

```python
ps aux|grep aria2c
```

![](http://images.weiphone.net/data/attachment/forum/201506/17/035039nxgjkenjzgsijix5.png)

出现图片中第三行的结果说明aria2c已经正常启动，如果无法正常启动，检查第二步中各种文件的位置是否正确。

###  第四步：通过webui-aria2控制aria2

aria2是基于命令行的下载工具，不过还好大神们早已开发了各种易用的UI方便我们小白们使用

最常用的webui-aria2: [http://ziahamza.github.io/webui-aria2/](http://ziahamza.github.io/webui-aria2/)

也可以用binux大神的YAAW：[http://binux.github.io/yaaw/demo/](http://binux.github.io/yaaw/demo/)

最简便的方法是直接用以上连接使用aria2c，（爱折腾的可以到[ https://github.com/ziahamza/webui-aria2 ]( https://github.com/ziahamza/webui-aria2 ) 或 [https://github.com/binux/yaaw ](https://github.com/binux/yaaw )下载所需文件自己搭建server）

下面以webui-aria2为例：
打开[http://ziahamza.github.io/webui-aria2/](http://ziahamza.github.io/webui-aria2/)
出现以下结果说明webui和aria2c已经连接成功

![http://images.weiphone.net/data/attachment/forum/201506/17/041956ckenbqydylqfykd8.png](http://images.weiphone.net/data/attachment/forum/201506/17/041956ckenbqydylqfykd8.png)


如果连接不成功可以打开Setting-Connection Setting查看host是否localhost, 端口是否是6800

![http://images.weiphone.net/data/attachment/forum/201506/17/041847gxveevme6vqxznv8.png](http://images.weiphone.net/data/attachment/forum/201506/17/041847gxveevme6vqxznv8.png)

到这里就可以在webui中添加连接或者种子开始下载啦

![http://images.weiphone.net/data/attachment/forum/201506/17/042520q7jgo6hz6ujfhymj.png](http://images.weiphone.net/data/attachment/forum/201506/17/042520q7jgo6hz6ujfhymj.png)

如果你以为教程到这里就结束了 那你就大错特错了
接下来才是最关键的步骤，导入迅雷离线和百度盘的任务aria2c


###  第五步：导入迅雷离线和百度盘下载

迅雷离线(需要迅雷会员)：
在chrome中下载binux大神的迅雷离线助手
[https://chrome.google.com/webstore/detail/thunderlixianassistant/eehlmkfpnagoieibahhcghphdbjcdmen?hl=zh-CN](https://chrome.google.com/webstore/detail/thunderlixianassistant/eehlmkfpnagoieibahhcghphdbjcdmen?hl=zh-CN)

![http://images.weiphone.net/data/attachment/forum/201506/17/043136cxgkxg58pgz58xq5.png](http://images.weiphone.net/data/attachment/forum/201506/17/043136cxgkxg58pgz58xq5.png)


迅雷离线中点右上角的小齿轮设置

![http://images.weiphone.net/data/attachment/forum/201506/17/043531crcfccx0xxczcxfp.png](http://images.weiphone.net/data/attachment/forum/201506/17/043531crcfccx0xxczcxfp.png)

会出现如下设置界面

![http://images.weiphone.net/data/attachment/forum/201506/17/043617eyb7cyit72002ic6.png](http://images.weiphone.net/data/attachment/forum/201506/17/043617eyb7cyit72002ic6.png)

确保path设置为： http://127.0.0.1:6800/jsonrpc
接下来就可以在迅雷离线中选中所需的任务，点批量导出中的YAAW，这样下载任务应该就成功导入aria2c了

![http://images.weiphone.net/data/attachment/forum/201506/17/043901poffzojyrfttyiiy.png](http://images.weiphone.net/data/attachment/forum/201506/17/043901poffzojyrfttyiiy.png)

again, 进入webui: [http://ziahamza.github.io/webui-aria2/](http://ziahamza.github.io/webui-aria2/) 查看任务是否添加成功

![http://images.weiphone.net/data/attachment/forum/201506/17/044516ifqyreklm5fy5syf.png](http://images.weiphone.net/data/attachment/forum/201506/17/044516ifqyreklm5fy5syf.png)

百度盘：chrome中下载BaiduExporter:
[https://chrome.google.com/webstore/detail/baiduexporter/mjaenbjdjmgolhoafkohbhhbaiedbkno](https://chrome.google.com/webstore/detail/baiduexporter/mjaenbjdjmgolhoafkohbhhbaiedbkno)

![http://images.weiphone.net/data/attachment/forum/201506/17/044903z7wfc7c07i76cyix.png](http://images.weiphone.net/data/attachment/forum/201506/17/044903z7wfc7c07i76cyix.png)

设置类似于迅雷

![http://images.weiphone.net/data/attachment/forum/201506/17/045116w933rxbh0zxjzt3n.png](http://images.weiphone.net/data/attachment/forum/201506/17/045116w933rxbh0zxjzt3n.png)

![http://images.weiphone.net/data/attachment/forum/201506/17/045156fimoynzni5vnlkjm.png](http://images.weiphone.net/data/attachment/forum/201506/17/045156fimoynzni5vnlkjm.png)

开机启动aria2方法：
把 /usr/local/aria2/bin/aria2c 这个文件拖入到开机启动项里即可

safari中添加迅雷离线和百度盘任务：
由于谷歌被墙，下不到chrome插件的小伙伴来看下面
迅雷离线:
safari里新建一个书签，网址填下面的内容，需要用插件的时候点在迅雷离线的页面中一下书签就可以启动，剩下的操作和chrome插件一样

```python
javascript:void((function(){var%20d=document;var%20s=d.createElement('script');s.src='http://s.binux.me/tle.js';s.id='TLE_script';d.body.appendChild(s)})())   
```

百度盘:
到下面网址下载safari的插件打开安装，剩下和chrome的插件一样
[https://github.com/acgotaku/BaiduExporter](https://github.com/acgotaku/BaiduExporter)
![http://bbs.feng.com/static/image/filetype/unknown.gif](http://bbs.feng.com/static/image/filetype/unknown.gif) [BaiduExporter.safariextz](https://pan.baidu.com/s/1o7EJ7CI)


  [1]: https://pan.baidu.com/s/1o7EJ7CI
