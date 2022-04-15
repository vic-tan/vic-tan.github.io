---
layout: page
title: "MOKA"
description: "谭乐言"
header-img: "img/zhihu.jpg"
---

# 代码提交与合并

###  RKT开发完成需同步到主分支(以2851M为例)
1、到自己服服器
2、cd进入2851M/kernel/android/R/vendor 目录下,如果是其它可以按下面几个步骤
2-1、如果提供sync code Excel表格，则先找path路径关键词 到git(http://10.126.16.60/gerrit/#/admin/projects/)搜索到项目路径，
2-2、找到对应的提交路径点 (gitweb)进入相对的文件目录-> 在heads 找到分支 点击shortlog
2-3、找到对应的提交代码点击 复制commit id
3、git log
4、查看需要同步提交的记录log (如果分支不对，先切换分支)（或者直接在git 搜索到的）
4-1、git checkout scbc/realtek/mac7p/android-11/scbc
4-2、git log 是否有上面提交的记录
4-3、git pull scbc realtek/mac7p/android-11/scbc
4-4、git log --oneline 查看是否有上面提交记录
4-5、git show 7e520f1d  查看修改内容并修改commit id(其它情况按上面步骤获得id
5、切回到主分支 git checkout scbc/rt51m/master
6、git log 确定分支切成功没有
7、合并修改的内容 git cherry-pick commit id  报错说明有冲突，自己手动改再提交
8、git push scbc HEAD:rt51m/master。



###  频道 DB 添加并同步到mastar 分支
注意：首先要确定当前分支，先从开发分支
1、到2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel目录下
2、到factorydata_app对应目录下，把tv.db 放到dvb目录下，如果是RTK的TV则是放到  rtk_provider_db目录下，否则放到provider_db目录下
3、到factorydata_vendor对应目录下，把channel下所有文件放到dvb/dtv_db目录下
4、到/2851M/kernel/android/R/device/tv051/R4/custom_imagesls
5、git log 查看一下当前目录及当前日志
6、git pull scbc realtek/merlin5/android-11/scbc 更新最新项目
7、git status 查看是否有冲突
8、git checkout ../..scbc.mk 有则回退
9、 git add  tclconfig/preset_channel/factorydata_app/2/ 添加文件夹
10、git status 查看是否添加
11、git commit -m "add rtk pl db tv" 添加提交信息
12、git push scbc HEAD:realtek/merlin5/android-11/scbc 提交到分支
13、git status 查看是否已提交
14、复制刚才提交的commit id
15、git checkout scbc/rt51m/master 切换到量产master 分支
16、git log 查看当前分支信息及日志信息
17、git pull scbc rt51m/master 同步当前分支
18、git status
19、git cherry-pick 2da8dd7f0f6c89042d4ee3eccacd503c33832bd5 同步开发分支commit id 文件
20、git status 确定是否同步成功


###  RTK代码sync code Excel提交同步分支相关路径
vendor_realtek_common_ATV_frameworks_native_ExtTv        2851M/kernel/android/R/vendor/realtek/common/ATV/frameworks/native/ExtTv
vendor_tv051_app           								 2851M/kernel/android/R/vendor/tv051/app
kernel_linux_linux-4.14_drivers_rtk_kdriver  			 2851M/kernel/linux/linux-4.14/drivers/rtk_kdriver

# TV 相关

手机下载应用试玩赚钱前几年就有了，我是最近才做。之所以推荐给大家是因确实能赚点零花钱，直接可能可以提现到微信和支付宝，也不需要我们交什么费用，不像有些兼职，需要交押金才可以。
这段时间以来，我下午只要有一空就会做试玩赚钱，由于我做了几十个平台，哪个有任务，我就做哪个平台，一会就能赚个几十元。

虽然试玩赚不了大钱，但却可以赚点零花钱，早餐钱，买菜钱，毕竟我们并不是指望它是我们主要来源，有事没事找开手机装几个软件即可以挣十几，总比天天拿着手机玩朋友圏，空间，微博来得实际些。

目前，我已经把各个平台的操作流程都了解的差不多了，而且几乎都差不多，都是复制关键词，搜索，下载，打开二三分钟就可以赚1到3元，非常的简单。

为了帮助更多想使用苹果手机赚钱的朋友，我把这些平台给弄一个排行榜，大家如果想做，可以按排行榜选择自己喜欢的软件来做。

1，以下二维码如果微信打不开，或提示升级等问题，直接选择在浏览器中打开就行，或用手机QQ扫描二维码下载。

2，有的下载后直接可以打开，有的得设置，如果是苹果依次打开设置－通用－描述文件与设备管理－企业级应用－找到所安装的软件，点击信任－再回到桌面就可以打开软件了。

3，如果你在苹果应用市场的帐号需要重新登录，发现密码输入错误，请到苹果商店官网输入邮箱，找回密码，重新设置下密码再登录就行了。

# 代码编译

如果你觉得不懂，可以点击以下链接查看新手教程（手把手教你,包含10分钟的短视频）。
*  <https://vic-tan.github.io/blog/2017/05/08/手机下载应用试玩赚零花钱新手教程/>

# 公司账号
*   扫一扫或识别下面二维码下载各平台试玩助手

*   试客小兵（应用试客）：一单价格2到3.5元，下午三点更新任务，10元提现。这个是最好的一个平台，任务简单，不需要注册，下载，打开几分钟就可以。
<center>
    <p><img   height="250" width="280" src="http://i1.piimg.com/593662/58ef5e21d448ff8b.jpg" align="center"></p>
</center>

*   神灯：一单1到2元，登录送2元，每天30多个快速任务，满10元可以微信和支付宝提现。
<center>
    <p><img   height="250" width="280" src="http://i4.buimg.com/593662/339ade217105bc48.png" align="center"></p>
</center>

*   鼠宝：一单1到2元，登录送2元，每天30多个快速任务，满10元可以微信和支付宝提现。
<center>
    <p><img   height="250" width="250" src="http://i1.buimg.com/593662/3ea1e4823b590da2.jpg" align="center"></p>
</center>

*   iMoney：加入送1元，满3元提现，一个任务的价格是1元左右，还可以点赞赚钱，不过得提升等级。
<center>
    <p><img   height="230" width="230" src="http://i1.piimg.com/593662/9986846b60adcd0e.jpg" align="center"></p>
</center>

*   PP红包：一单1到2元，登录送3元，每天30多个快速任务，满10元可以微信和支付宝提现。
<center>
    <p><img   height="420" width="280" src="http://i2.muimg.com/593662/de0a9d942ec7c0c7.jpg" align="center"></p>
</center>


*   钱庄：登录送奖励，一单价格是一元左右，独家任务多，9，14，16点更新任务，下载任务比较多，满5元可以微信提现。
<center>
    <p><img   height="280" width="280" src="http://i2.muimg.com/593662/584304546eba610e.jpg" align="center"></p>
</center>

*   小鱼：登录送奖励，一单价格是一元左右。
<center>
    <p><img   height="280" width="280" src="http://i1.buimg.com/593662/cb8e1eecce8a5131.jpg" align="center"></p>
</center>

*   掉钱眼儿：登录送奖励，一单价格是一元左右。
<center>
    <p><img   height="420" width="280" src="http://i4.piimg.com/593662/228b7943767df16b.png" align="center"></p>
</center>


*   懒猫：一单1到2元，登录送2元，每天30多个快速任务，满10元可以微信和支付宝提现。
<center>
    <p><img   height="250" width="280" src="http://i4.buimg.com/593662/391078944926208a.jpg" align="center"></p>
</center>


*   赚客：签到可得50金币，一个任务的价格是1元左右，满5元可提现。
<center>
    <p><img   height="420" width="280" src="http://i1.piimg.com/593662/51c6eae5ef3437f6.jpg" align="center"></p>
</center>

*   红包达人：一单1到2元，登录送2元，每天30多个快速任务，满10元可以微信和支付宝提现。
<center>
    <p><img   height="250" width="280" src="http://i4.buimg.com/593662/c79a75faa4fe4678.png" align="center"></p>
</center>


*   内涵红包：安卓、苹果、ipad可用热葫芦：注册送2元，1个任务是1到3元左右，满10元提现。
<center>
    <p><img   height="230" width="230" src="http://i1.piimg.com/593662/81a5072b323a7a79.jpg" align="center"></p>
</center>




上面这些试玩赚钱APP我都提现过，都是真实可靠的，所以，大家可以放心去做。

上面都是不错的平台，不会轻易的封号，这才是最主要的，只要这个平台能够提现到帐，那就是好平台。对于一些平台恶意封号，价钱再高我们也不要做。

苹果手机试玩，开始一天能赚几十元，不过后来赚的就越来越少了，所以，大家可以多下载一些软件。我们也不要指望这个能赚多少，多少钱，只不过当个兴趣，没事做几个，赚点零花钱就行了。

许多人都想找个兼职，可是，找来找去也不知道干什么好。而试玩APP，就当作是我们的一个兼职，没事的时候做点任务，赚点外块，就这么简单，最主要的是价格高，简单，提现快。

试玩赚钱APP，只要存在一天，我就会做一天，因为对于手机赚钱来说，是非常不错的一种兼职。大家在开始做的时候，肯定有不会，不懂的，不过，只要操作的时间长了就会了，也希望大家能通过苹果手机赚到更多的钱。


# 相关路径

如果你觉得真如我所说，请你把本文分享给你周边的朋友，大家都受益，或者扫下面的二维码进行分享好友 ，或者复制下链接分享。

*  <https://vic-tan.github.io/milestone/>

*  分享二维码
<center>
    <p><img   height="280" width="230" src="http://i4.buimg.com/593662/62f555d0e3fb0102.png" align="center"></p>
</center>
