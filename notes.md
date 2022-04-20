---
layout: page 
title: "notes"
header-img: "img/zhihu.jpg"
---
### 笔记

## <-----------------------代码提交与合并-------------------——>
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
> * 7、合并修改的内容 git cherry-pick commit id 报错说明有冲突，自己手动改再提交
> * 8、git push scbc HEAD:rt51m/master。

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
> * 22、如果提示pull 则先回退到上一步 git reset --hard 
> * 23、repo sync . 同步所有文件
> * 24、又重新从第6步开始



### RTK代码sync code Excel提交同步分支相关路径

> * vendor_realtek_common_ATV_frameworks_native_ExtTv        
    * 2851M/kernel/android/R/vendor/realtek/common/ATV/frameworks/native/ExtTv
> * vendor_tv051_app           								 
    * 2851M/kernel/android/R/vendor/tv051/app
> * kernel_linux_linux-4.14_drivers_rtk_kdriver  			 
    * 2851M/kernel/linux/linux-4.14/drivers/rtk_kdriver 


## <--------------------------TV 相关----------------------——>
### 启动工厂DesignMenuActivity界面
> * am start -n com.apps.factory.ui/.designmenu.DesignMenuActivity

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
> * 5、 把文件复制到U盘 cp RMCA_ready /mmnt/udisk/sda1/  U盘路径/mmnt/udisk/开头


### 串口与adb 端口冲突时，可以先把日志写到文件再打开串口
> * logcat -s FactoryUart > /data/temp/log.log

### 串口占用时先保存要查看日志，再占用
> * logcat -s  tag > data/log.log

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

### 反抄写
> * 1、把反抄写文件放到U盘根目录下
> * 2、上电一直按着ESC按住进bootcode，显示Realtek>后
> * 3、输入 mp_restore


### 工厂遥控器
> * 1、长按APT 等上面两个灯长亮后
> * 2、如果是RCA协议按000 ，NEC协议按001 ,松下按010


## <--------------------------代码编译----------------------——>
### 整个软件编译
> * 1、进入项目级目录(如2851M)
> * 2、./
> * 3、./scbc_build_51m.sh
> * 4、回车
> * 5、2
> * 6、n
> * 7、n
> * 8、第一次y以后n
> * 9、一直回车
> * 10、输出 Image creation complete. Output file:install_wipe.img 为正常
> * 11、输出目录为根目录(如2851M)2851M/Buildimg/V8-T851MGL-LF1V001

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

### 预设频道路径
> * \mnt\verdor\tclcofig\model
> * /home/lifei/2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel



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


## <--------------------------常用git 命令----------------------——>
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

