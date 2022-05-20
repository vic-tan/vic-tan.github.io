---
layout: page 
title: "notes"
header-img: "img/zhihu.jpg"
---

## -----------------------------------提交代码----------------------------

### 编译整个软件
***

> 1. 连接进入自己的服务器
> 2. 命令进入需要编译的项目， eg: cd 2851M
> 3. 确保项目环境干净，防止出现因之前切换过分支导致环境不干净，先清除环境，输入如下清除命令 repo forall -c "pwd && git clean -xfd && git checkout -- ."
> 4. 确定要编译的分支，然后init编译的分支,可以在以下查找分支
> + [2841A分支管理链接](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35542944)
> + [2851M分支管理链接](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632)
> + 输入init命令 eg:repo init -u ssh://10.126.16.60:29418/rt51M_manifest -m odin-gms.xml -b realtek/merlin5/android-11/scbc
> 5. 同步当前分支代码 命令:repo sync -j8
> 6. 同步过程如果提示报错文件，可以先删除该文件，然后重新从2步开始，还是不行可以直接删除R3或R4文件，再重新第2步开始
> + 删除文件命令 eg:rm -rf kernel/android/R/vendor/R4
> 7. 执行脚本 ./scbc_build_51m.sh  回车
> + 文件一定是在项目根目录下执行，在根目下，可以打前几个然后tab 出来 
> + 在脚本后面加 0 true 表示公版 eg:./scbc_build_51m.sh 0 true
> 8. 请输入版本号[Please type version] 按回国为默认V8-T851MGL-LF1V001版本号，最好按时间命名加上自己的标签，ge:V8-T851MGL-LF1V20220421_TAN
> 9. CTS or not?[1-CTS 2-Debug 3-CTS&Debug] 表示编译软件类型，一般输入 2
> + 1-CTS，表示用户模式，有很多权限限制
> + 2-Debug，表示开发模式，有root等多个权限，适合开发
> 10. Build OTA package or not? 表示是否支持OTA升级，一般输入 n
> 11. 是否启用远场语音[n/y]? 表示当前软件是否支持远场语音，一般输入 n，41A硬件不支持这项
> 12. Clean or not? 选n 是否先清除下环境
> 13. 请输入客户品牌[Please type product][0-NOKIA(Default) 1-MOTOROLA 2-NOKIA_2K 3-PANASONIC_EU 4-PANASONIC_FFM 5-PANASONIC_BASE] 品牌选择（1，2 表示区域客户，3、4表示松下）
> 14. 回车后，如果是清空环境第一次编译，一般要等待2个小时，已经编译过第二次，则基本上30分钟。
> 15. 编译完成后会输出 Image creation complete. Output file:install_wipe.img 为正常
> 16. 编译完成后会输出路径为根目录 eg:2851M/Buildimg/V8-T851MGL-LF1V20220421_TAN



## -----------------------代码提交与合并-------------------—
### sync code Excel表格 找到对应的服务器下路径

> * 1、方法一（推荐）
    * 1-1、打开sync code Excel提交文件在我们git 服务器路径。
    * 1-2、在信息行中，找到tree行点击tree。
    * 1-3、复制随便找一个提交文件名：如TvApiHooker（不带后缀）。
    * 1-4、打开服务器，找到对应的项目下如：2851M。
    * 1-5、打开.repo文件夹。
    * 1-6、打开manifests文件夹。
    * 1-7、打开mac7p-atv-scbc.xml文件。
    * 1-8、搜索TvApiHooker，对应的path便是对应的路径。

> * 2、方法二
    * 2-1、打开sync code Excel提交文件在我们git 服务器路径。
    * 2-2、在信息行中，找到tree行点击tree。
    * 2-3、复制随便找一个提交文件名：如TvApiHooker.cpp。
    * 2-3、rtk 文件一般在/kernel/android/R/vendor/realtek/common/ATV 下，先cd到目录
    * 2-4、输入命令 find -name "TvApiHooker.cpp"。
    * 2-5、等一会会搜索到对应的路径./frameworks/native/ExtTv/src/TvApiHooker/TvApiHooker.cpp。

### patch 文件代码合并命令
> * 1、选择打开patch 文件，找到要patch 文件路径 Subject: &#91;PATCH&#93;
> * 2、在自己的服务器上cd 到该目录下，把patch 文件放到该目录下
> * 3、执行patch -p1 < ？ 命令 ？ 表示patch 文件全名，包括后缀 
    * eg:  patch -p1 <0001-kernel-android-R-vendor-realtek-common-ATV-app-RtkTv.patch
> * 4、如果出Hunk #2 succeeded at 149 (offset 5 lines). 说明有冲突 
    * 如果有冲突，会多出一个.orig的文件。如 :AndroidManifest.xml.orig （orig 为之前的文件）
    * git diff 冲突文件查看具体修改（或者用对比工具比较）
    * 如:git diff /home/lifei/2851M/kernel/android/R/vendor/realtek/common/ATV/app/RtkTvProvider/AndroidManifest.xml
    * 对比是不是patch 要修改的内容，进行修改，然后删除 rm -rf AndroidManifest.xml.orig
     

### diff 文件代码合并命令方法
> * 1、选择打开diff 文件，找到要diff 文件路径 diff -&#45;git &#91;PATCH&#93;
> * 2、在自己的服务器上cd 到该目录下，把diff 文件放到该目录下
> * 3、执行git apply 文件路径(可以tab 出来)


### 切分支切换，确保环境代码干净
> * 1、如果切分支支不到对应的分支，或者有错误
> * 2、repo forall -c "pwd && git clean -xfd && git checkout -- ."
> * 3、rm -rf R4 （删除R4 或者R3）
> * 4、repo init -u 切到对应的分支
> * 5、repo sync -j8 （同步）

### RKT开发完成需同步到主分支(以2851M为例)

> * 1、到自己服服器
> * 2、cd进入2851M/kernel/android/R/vendor 目录下,如果是其它可以按下面几个步骤
    * 2-1、如果提供sync code Excel表格，则先找path路径关键词到git路径[http://10.126.16.60/gerrit/#/admin/projects/](http://10.126.16.60/gerrit/#/admin/projects/)搜索到项目路径
    * 2-2、找到对应的提交路径点 (gitweb)进入相对的文件目录-> 在heads 找到分支 点击shortlog
    * 2-3、找到对应的提交代码点击 复制commit id
> * 3、git log
> * 4、查看需要同步提交的记录log (如果分支不对，先切换分支)（或者直接在git 搜索到的）
    * 4-1、git checkout scbc/realtek/mac7p/android-11/scbc
    * 4-2、git log 是否有上面提交的记录
    * 4-3、git pull scbc realtek/mac7p/android-11/scbc
    * 4-4、git log --oneline 查看是否有上面提交记录
    * 4-5、git show 7e520f1d 查看修改内容并修改commit id(其它情况按上面步骤获得id
> * 5、切回到主分支 git checkout scbc/rt51m/master
> * 6、git log 确定分支切成功没有
> * 7、合并修改的内容 git cherry-pick commit id 报错说明有冲突，自己手动改再提交 ，如果报HEAD detached from 8a3c346 You are currently cherry-picking commit cde479c.有可能是有人同步过了
> * 8、git add .
> * 9、git cherry-pick -&#45;continue
> * 10、可以执行git cherry-pick -&#45;abort取消上次操作
> * 11、git push scbc HEAD:rt51m/master。

### 频道 DB 添加并同步到mastar 分支

注意：首先要确定当前分支，先从开发分支

> * 1、到2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel目录下
> * 2、到factorydata_app对应目录下，把tv.db 放到dvb目录下，如果是RTK的TV则是放到 rtk_provider_db目录下，否则放到provider_db目录下
> * 3、到factorydata_vendor对应目录下，把channel下所有文件放到dvb/dtv_db目录下
> * 4、到/2851M/kernel/android/R/device/tv051/R4/custom_images ls
> * 5、git log 查看一下当前目录及当前日志
> * 6、git pull scbc realtek/merlin5/android-11/scbc 更新最新项目
> * 7、git status 查看是否有冲突
> * 8、git checkout ../..scbc.mk 有则回退之前的修改
> * 9、git add tclconfig/preset_channel/factorydata_app 添加文件夹
> * 10、git status 查看是否添加 
> * 11、git commit -m "add rtk pl db tv" 添加提交信息 
> * 12、git push scbc HEAD:realtek/merlin5/android-11/scbc 提交到分支
> * 13、git status 查看是否已提交 
> * 14、复制刚才提交的commit id 
> * 15、git checkout scbc/rt51m/master 切换到量产master 分支
> * 16、git log 查看当前分支信息及日志信息 
> * 17、git pull scbc rt51m/master 同步当前分支 
> * 18、git status 
> * 19、git cherry-pick 2da8dd7f0f6c89042d4ee3eccacd503c33832bd5 同步开发分支commit id 文件
> * 20、git status 确定是否同步成功
> * 21、git push scbc HEAD:rt51m/master。
> * 22、如果提示push 失败要先pull， 则先回退到上一步 git reset -&#45;hard 
> * 23、repo sync . 同步所有文件
> * 24、又重新从第6步开始



### RTK代码sync code Excel提交同步分支相关路径

> * vendor_realtek_common_ATV_frameworks_native_ExtTv        
    * 2851M/kernel/android/R/vendor/realtek/common/ATV/frameworks/native/ExtTv
> * vendor_tv051_app           								 
    * 2851M/kernel/android/R/vendor/tv051/app
> * kernel_linux_linux-4.14_drivers_rtk_kdriver  			 
    * 2851M/kernel/linux/linux-4.14/drivers/rtk_kdriver 
> * ExtTv.java
    * kernel\android\R\vendor\tv051\app\exttv-framework\core\java\com\exttv\tv



## -------------------------TV 相关----------------------—
### 启动Activity界面
> * am start -n com.apps.factory.ui/.designmenu.DesignMenuActivity //工厂菜单 
    * 1-1、设置主界面->Picture & Display -> Picture Adjustment ->Advanced Settings->Brightness -> 选中Contrast ->按数字1950
    * 1-2、设置主界面->Picture & Display -> Picture Adjustment ->Advanced Settings->Brightness -> 选中Contrast ->连按OK五下
> * am start -n com.apps.tvsettings/com.apps.tvsettings.ui.MainSettings //设置主界面
> * am start -n  com.apps.livetv/.SelectInputActivity   // 信源菜单
> * am start -n com.apps.atv/.atvsetup.AnalogueSetupActivity   //ATV搜台 
> * am start -n com.realtek.tv.atv/.atvsetup.AnalogueSetupActivity   //RTK ATV搜台
> * am start -n com.apps.dtv/.DigitalSetup.DigitalChannelSetupActivity     //DTV搜台
> * am start -n com.realtek.dtv/.DigitalSetup.DigitalChannelSetupActivity     //RTK DTV搜台


### 修改TV系统文件申请权限（增删改）
> * 1、开机ESC按住进bootcode，显示Realtek>后
> * 2、env set OEMLock off
> * 3、env save
> * 4、reset
> * 5、打开用adb工具
> * 6、adb connect 192.168.1.128 （windowns :adb connect 192.168.1.128:5555 ）
> * 7、adb root
> * 8、adb disable-verity
> * 9、adb reboot
> * 10、adb connect 192.168.1.128
> * 11、adb root
> * 12、adb remount
> * 走完以上步骤即可以对系统文件增删改了



### CTS 版本文件查看权限

> * 1、上电长按tab（不小心按错了长按了Esc建 可以 输入go r）
> * 2、创建文件夹（要查看哪个目录就用以下那个）
    * mkdir /mnt/vendor/tclconfig
    * mkdir /mnt/vendor/impdata
> * 3、挂载（把要看的文件挂载出来）（这个？号根据机型不同可能名字不同）
    * mount -t ext4 /dev/block/？ /mnt/vendor/tclconfig
    * ？号值可以通过命令 ls -l dev/block/by-name/查询
    * 2851M
    * mount -t ext4 /dev/block/mmcblk0p31 /mnt/vendor/tclconfig
    * mount -t ext4 /dev/block/mmcblk0p33 /mnt/vendor/tvdata
    * mount -t ext4 /dev/block/mmcblk0p34 /mnt/vendor/impdata
    * mount -t ext4 /dev/block/mmcblk0p4 /mnt/vendor/factory
    * mount -t ext4 /dev/block/mmcblk0p6 /mnt/vendor/factory_ro
    * mount -t ext4 /dev/block/mmcblk0p30 /mnt/vendor/tvdata
    * 2841A
    * mount -t ext4 /dev/block/mmcblk0p29 /mnt/vendor/tclconfig
    * mount -t ext4 /dev/block/mmcblk0p31 /mnt/vendor/impdata
    * mount -t ext4 /dev/block/mmcblk0p4 /mnt/vendor/factory
    * mount -t ext4 /dev/block/mmcblk0p6 /mnt/vendor/factory_ro
    * mount -t ext4 /dev/block/mmcblk0p30 /mnt/vendor/tvdata

> * 4、如果插上U盘一直有输出日志打印，输入ps 命令查看所有进程，然后可以kill 109 进程（/sbin/loader_m）
> * 5、把文件复制到U盘 cp RMCA_ready /mmnt/udisk/sda1/  U盘路径/mmnt/udisk/开头(文件夹cp -rf * /mmnt/udisk/sda1/)
> * 6、sync


### 盲切ID
> * 按遥控器0 6 2 5 9 8 MENU +ID    注：（ ID10 是010  ID 是三位数）

### 键盘切ID
> * 062598 menu 047
注意：前面这数字要用一排的那行数字输入
id 是三位数

### 改屏参
> * 连接串口->断电上电->长按ESC(出现Realtek)->panel->选择序号->re(重启)

### TV系统升级
> * 连接串口->reboot->长按tab等待日志出来

### 去掉遥控器屏蔽串口命令
> * settings put global shop_ir_lock 0
 
### 删除文件
> *  rm -rf R4（R4 表示文件名字）

### 从U盘复制文件到当前路径
> * cp /mnt/media_rw/00AF-9C6B/RMCA_ATV/RMCA .

### 设置tclconfig 权限
> * mount -o remount,rw /mnt/vendor/tclconfig

### 反抄写
> * 1、把反抄写文件放到U盘根目录下
> * 2、上电一直按着ESC按住进bootcode，显示Realtek>后
> * 3、输入 mp_restore


### 工厂遥控器
> * 1、长按APT 等上面两个灯长亮后
> * 2、如果是RCA协议按000 ，NEC协议按001 ,松下按010


### 查看用audio fw
> * 1、进kernel\system\configs文件夹
> * 2、找到对应的品牌配置
> * 3、打开找到AUDIO_ADDNAME ="_tv051_jbl" 后面就是用的aduio fw 名字的后缀
> * 4、进kernel\fw\audio_fw\4K文件夹 bluecore.audio.text_+ 上的后缀 ,即bluecore.audio.text_tv051_jbl.zip

## --------------------------串口相关 ----------------------—

### PASS 返回值
> * PASS
    * 固定值 AB 05 0A DF 4E （0A 为pass OE或OF 为Fail）

## --------------------------logcat ----------------------—

### 串口与adb 端口冲突时，可以先把日志写到文件再打开串口
> * logcat -s FactoryUart > /data/temp/log.log

### 串口占用时先保存要查看日志，再占用
> * logcat -s  tag > data/log.log

### Grep -E这个参数过滤多个关键字
> * logcat &#124; grep -E "word1 &#124; word2 &#124; word3"

## --------------------------代码编译----------------------


### 单个项目编译
> * 1、进入项目R文件级
> * 2、source build/envsetup.sh
> * 3、lunch
> * 4、找到对应项目序号(2851M 序号3 )
> * 5、lunch 3
> * 6、进入需要编译的目录(如kernel/android/R/vendor/realtek/common/ATV/app/RtkTvProvider)
> * 7、mm -j32

### 编译中间件TVMidwareManager
> * 1、repo init -u ssh://10.126.16.60:29418/rt51M_manifest -m odin-gms.xml -b realtek/merlin5/android-11/scbc
> * 2、repo sync -j8 (同步)
> * 3、到2851/目录下 repo sync .
> * 4、到/2851M/kernel/android/R目录下
> * 5、source build/envsetup.sh
> * 6、lunch 3
> * 7、把整个TVMidwareManager项目拷贝到/2851M/kernel/android/R/vendor/tv051/app/rtk_app下
> * 8、拷贝完后cd 到/2851M/kernel/android/R/vendor/tv051/app/rtk_app/TVMidwareManager下
> * 9、 mm -j32
> * 10、编译完成后输出 Install: /2851M/kernel/android/R/out/target/product/R4/system_ext/framework/tv-midware-manager.jar


### 中间件更新步骤
> * 中间件路径 https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/TVMidware/debug/[2851M]
    * 1、下载以上最新tv-midware-manager.jar文件push 到TV上 /system_ext/framework目录
    * 2、下载以上最新TVMidwareService.apk文件push 到TV上 /system_ext/app/TVMidwareService 目录

### 本地项目打包apptvmidware.jar步骤
> * 1、打开中间件项目，更新最新代码
> * 2、在左下角的Buiild Variants : apptvmidware 选择 debug
> * 3、选中apptvmidware项目->Build ->make module
> * 4、右上角Gradlle->apptvmidware项目 ->Tasks->Other->makeJar->会在build->lib下生成apptvmidware.jar包 



## <--------------------------相关路径----------------------——>
### 所有APP项目总路径
> * https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src

### 工厂菜单项目SVN路径
> * https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/Factory_General

### 中间间项目SVN路径
> * https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/TVMidware

### 项目档案
> * https://odm-design-center-hz.tclking.com/svn/Project_Document

### 项目抄Key址址
> * https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2851M安卓TV机芯项目档案
> * https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2841A机芯项目档案
> * https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2841机芯项目档案
> * https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2851机芯项目档案

### 红屏断言
> * https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/LogApp

### 2851M代码管理
> * [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632)

### 流程规范
> * 1、软件设计规范
> * [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60696466](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60696466)

### 应用&中间件 ReleaseNote
> * [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=47625057](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=47625057)






### 工厂菜单apk安装路径
> * adb push /Users/tanlifei/Desktop/work/moka/project/Factory_General/app/release/Factory.apk /system_ext/app/Factory

### 日志apk安装路径
> * adb push /Users/tanlifei/Desktop/work/moka/project/LogApp/app/release/LogApp.apk /system_ext/app/LogApp

### 中间件服务安装路径
> * adb push /Users/tanlifei/Desktop/TVMidwareService.apk /system_ext/app/TVMidwareService

### 中间件代码安装路径
> * adb push /Users/tanlifei/Desktop/tv-midware-manager.jar /system_ext/framework

### 共享目录
> * \10.126.69.28（2841A）
> * \10.118.1.100（2851M）


## -------------------------常用git 命令----------------------
### 查看时间范围内的提交日志
> * git log --after="2022-3-1" --before="2022-5-1" --pretty=format:"%an %ad : %s" --date=short --no-merges --reverse
    * 参数	含义
    * –author	作者
    * –committer	提交者
    * –after	某时间后
    * –before	某时间前
    * –reverse	按时间顺序
    * –grep	提交说明包含字符串
    * -S	修改内容包含字符串
    * –pretty	格式化信息	oneline、short、full、fuller、format
    * –date	日期格式	relative、local、default、iso、rfc、short、raw
    * –no-merges	隐藏合并提交

### 回退版本
> * 1、git revert ID

### 查看DailyBuild 编译code 对应的版本
> * 1、打开编译DailyBuild 文件夹 eg:DailyBuild_RT2851M_0428
> * 2、打开save_apk_svn
> * 3、搜索自己要找的apk名
> * 4、找到Last Changed Rev: 14784
> * 5、14784 即为SVN 代码打包的代码


### NPI项目
> * TVN21-A-023 日本I客户55A5000 1RT851E1ISA COST ST5461D12-6 TV项目 （日本安卓R真4K智能电视）（李佳和-日规）
> * TVN21-A-089 东欧松下75D8310L 1RT51MB1T2A CSOT ST7461D03-2 TV项目（台湾）（SPM 龙飞扬-松下HDR10+）
> * TVN21-C-067 东欧松下43D8300P 1RT51MC1S2B 配INX V430DJ1-Q01 D2 TV项目(新西兰）（SPM 龙飞扬-松下远场语音）
> * TVN21-C-083 东欧松下50D8300P 1RT51MC1ISA配INX V500DJ7-QE1 (D5) TV项目（菲律宾）（SPM 龙飞扬-泰霖）
> * TVP22-E-002 北非Stream客户43D6100F 5RT41AB1S2A HKC PT430CT02-4 TV派生项目 (销往欧洲 CVTE机芯）（SPM 骆鑫-CVT）
> * TVP21-L-009 南美Solnik客户 50D2090 1RT851A1ISA CHOT CV500U2-T01 V02 TV配屏项目（阿根廷)（SPM 陈嘉平）
> * TVN22-C-023 战略P客户 32D2030 5RT41AB1S2A CSOT ST3151A07-2 V2.7 TV DVB-S2&T2项目（非洲TH-32LS670MF）（SPM 赵龙）


## -----------------------常用账号-------------------

### Termius 账号
> * 1、Termius 账号179840045@email.top (版本7.34.1可重复用0045可以累加，密码随便)

### SPM版本发布releaseNote路径
> * https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60693752

### 查找2851M批次及发布软件时间
> * 1、把批次进入该网址查找对应的软件版本[https://rd-mokadisplay.tcl.com/rdm/](https://rd-mokadisplay.tcl.com/rdm/)
> * 2、打开2851M软件发布列表打到发布时间即可[https://docs.qq.com/sheet/DZUNJUWpWdVFVcWNO?tab=BB08J2](https://docs.qq.com/sheet/DZUNJUWpWdVFVcWNO?tab=BB08J2)

### 查找2841A批次及发布软件时间
> * 1、把批次进入该网址查找对应的软件版本[https://rd-mokadisplay.tcl.com/rdm/](https://rd-mokadisplay.tcl.com/rdm/)
> * 2、打开2841A软件发布列表打到发布时间即可[https://docs.qq.com/sheet/DTEtSY3FVWnVjYWxv](https://docs.qq.com/sheet/DTEtSY3FVWnVjYWxv)

## -------------------db 工厂预设频道路径----------------

### 预设频道路径
> * 1、确定客户的制式与工厂ID，提供db数据,必须有三个db文件和channel 文件夹
> * 2、进入自己服务/home/lifei/2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel目录下
> * 3、到factorydata_app对应目录下，找对应的工厂id 文件夹后，找对就的制式文件夹，进入dvb目录下，这三个文件夹没有则新建文件夹
> * 4、把tv.db 放到 rtk_provider_db还是provider_db目录下，具体放哪个文件，按以下确定
    * 区分rtk_provider_db目录还是provider_db 目录 ，把客户提供的DB 打，查看数据中的包名是否带ealtek（rtk），带则把db放到rtk_provider_db目录下，否则放provider_db 目录下
> * 5、打factorydata_vendor对应目录下，把channel下所有文件放到dvb/dtv_db目录下（如果dtv_db已经存在，有数据，可以不用在传，rtk_provider_db和provider_db可以共用一套）
> * 工厂所有信息如下：
    *|序号 |工厂 |  代号（数据库名）|
    | :----- | :---- | :---- |
    |1| 惠州工厂|HZ|                                 
    |2| 波兰工厂| PL|
    |3| TTET工厂| CD|
    |4| 无锡工厂| WX|
    |5| 内蒙古工厂| NM|
    |6| 惠州ODM工厂| HZ_ODM|
    |7| 越南工厂| YN|
    |8| 泰国工厂| TH|
    |9| 菲律宾工厂| FLB|
    |10| 印度尼西亚工厂| IND|
    |11| 俄罗斯工厂| RU|
    |12| 埃及工厂| EG|
    |13| 捷克工厂| JAC|
    |14| 巴基斯坦工厂| PAKISTAN|
    |15| BENNE工厂| BENNE|
    |16| ARABY工厂| ARABY|
    |17| ELTHOLATHIA工厂| ELTHOLATHIA|
    |18| 突尼斯工厂| TUN|
    |19| 刚果工厂| CONDOR|
    |20| UPAC工厂 |UPAC|
    |21| 俄罗斯 PKV 工厂| PKV|
    |22| 东莞理想工厂| DG|
    |23| 苏州乐轩工厂| SZ|
    |24| 惠州TOT工厂 |TOT|
    |25| 阿尔及利亚工厂 |ALGERIA|
    |26| CVT工厂 |CVT|
    |27| 南非工厂| RSA|
    |28| 泰国ORION工厂 |ORION|
    |29| 墨西哥工厂 |MX|
    |30| 阿根廷RV工厂| RV|
    |31| 巴西SEMP工厂| SEMP|
    |32| 印度工厂 |INDIA|
    |33| 哥伦比亚工厂 |CB|
    |34| 阿根廷NEWSAN工厂 |NEWSAN|
    |35| 印度DIXON工厂 |DIXON|
    |36| 阿根廷SOLNIK工厂| SOLNIK|
    |37| 乌兹别克斯坦工厂 |UZB|
    |38| 俄罗斯KVANT工厂 |RU_KVANT|
    |39| 阿根廷ELECTROFUEGUINA工厂| ELECTROFUEGUINA|
    |40| 松下马来西亚工厂 |PANASONIC_MALAYSIA|
    |41| 伊朗工厂 |IRAN|
    |42| 俄罗斯TELEBALT工厂 |RU_TELEBALT|
    |43| 印度TIRUPATI工厂 |TIRUPATI|
    |44| 松下墨西哥PANAMEX工厂| PANAMEX|
    |45| 松下俄罗斯HORIZONT工厂 |HORIZONT|
    |46| 北非ELARABY工厂 |ELA|
    |47| 印度ONIDA工厂| ONIDA|
    |48| 泰霖TL工厂 |TL|
    |49| MAXI工厂|MAXI|
    |50| 乌兹别克ARTEL客户工厂 |ARTEL|
    |51| 土耳其ARCELIK工厂 |ARCELIK|
    |52| 巴西松下Panabras客户工厂 |PANABRAS|
    |53| 印度VIDEOTEX工厂 |VIDEOTEX|
    |54| 印度Genus工厂 |GENUS|
    |55| 印度Veira工厂 |VEIRA|
    |56| 印度Micromax工厂 |MICROMAX|
    |57| 乌兹别克ARTEL_SMT客户工厂 |ARTEL_SMT|
    |58| 俄罗斯Videoton工厂 |VIDEOTON|
    |59| 增加泰国TASTH009工厂 |TASTH009|
    |60| 松下PTW客户工厂 |PTW|
    |61| 印度MOKA工厂 |MOKA|
    |62| 印度ACER客户工厂 |ACER|