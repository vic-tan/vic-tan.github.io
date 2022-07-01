---
layout: page 
title: "笔记"
header-img: "img/zhihu.jpg"
---

## -----------------------------------提交代码----------------------------  

***
### 编译整个软件
***

> 1. 连接进入自己的服务器 eg 10.126.69.28
> 2. 命令进入需要编译的项目， eg: cd 2851M
> 3. 确保项目环境干净，防止出现因之前切换过分支导致环境不干净，先清除环境，输入如下清除命令 repo forall -c "pwd && git clean -xfd && git checkout -- ."
> 4. 确定要编译的分支，然后init编译的分支,可以在以下查找分支
> + [2841A分支管理链接](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35542944)
> + [2851M分支管理链接](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632)
> + 输入repo init -u patch 命令 eg:repo init -u ssh://10.126.16.60:29418/rt51M_manifest -m odin-gms.xml -b realtek/merlin5/android-11/scbc
> 5. 同步当前分支代码 命令:repo sync -j8
> 6. 同步过程如果提示报错文件，可以先删除该文件，然后重新从2步开始，还是不行可以直接删除R3或R4文件，再重新第2步开始
> + 删除文件命令 eg:rm -rf kernel/android/R/vendor/R4
> 7. 执行脚本 ./std-build  或者./scbc_build_51m.sh  回车（如果有std-build则用std-build）
> + 文件一定是在项目根目录下执行，在根目下，可以打前几个然后tab 出来 
> + 在脚本后面加 0 true 表示公版 eg:./scbc_build_51m.sh 0 true
> + 此过程会自动去下载关联apk，如果此时想编译自己本的apk，工具日志下载完apk后就可可以直接替换自己的apk到对应的目录下，这样编译出来的就自己的apk。apk 路径一般在kernel/android/R/vendor/tv051/app/prebuilt_app/目录下
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


***
### 同步开发代码
***  

> 1. 连接进入自己的服务器 eg : 10.126.69.28
> 2. 命令进入需要编译的项目， eg: cd 2851M
> 3. 切换到开发分支,可执行repo init -u patch 切到对应分支，或者cd 到有仓库的文件目录下git checkout 切换分支
> 4. 同步当前分支代码 命令:repo sync -j8
> 5. cd 到需要同步代码的仓库
> + 预置DB路径 ：2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel
> + RTK 同步Code 仓库路径请看下面RTK 同步Code 代码的仓库查找说明纪要
> 6. git pull scbc realtek/merlin5/android-11/scbc 更新当前目录或者（repo sync .）
> 7. git status 查看当前工作区否干净
> 8. git log 或者git log -&#45;oneline查看日志并检查当前分支是否正确
> 9. 复制要同步的commit id 
> 11. git branch -a 查看当前仓库所有分支 
> 12. git checkout 要同步的分支 eg git checkout scbc/rt51m/master 
> 13. git log 确定一下分支是否切换成功
> 14. git cherry-pick ID ，ID 为复制的要同步的commit id
> 15. 如果报错报黄说明有冲突，需要找到冲突文件，修改冲突文件内容，再提交 
> 16. git status 此时工作区否为你修改冲突的文件
> 17. git add 文件名 
> + 如果是提交操作，需要走git commit -m "add rtk pl db tv" 添加提交信息 然后走20步开始即可
> 18. git checkout ../..scbc.mk 当前可以回退之前的修改
> 18. git cherry-pick -&#45;continue 继续同步
> 19. 如果想取消同步，可以执行git cherry-pick -&#45;abort取消上次操作
> 20. git push scbc HEAD:rt51m/master 把同步到本地的代码提交到远程服务
> 21. 如果提示push失败，提示要先pull， 则先回退到上一步 git reset -&#45;hard 重新从6步开始 


***
### RTK 同步Code 代码的仓库查找
***  

> 1. 一般RTK 会提供一个 sync code Excel表格
> 2. 打开sync code Excel提交文件
> 2. 在表格中找到路径栏，一般文件夹是以_，从左到右即为服务器中的路径
> 3. 如果在路径栏无法直接定位路径，可以按下面方法寻找
> 4. 方法一（推荐）
> + 找到Excel 中的最一栏，以http://10.126.16.60/ 开头的路径，这个是git提交路径 
> + 把开找到的链接，在信息行中，找到tree 文字，然后行点tree 进入。
> + 找到这笔提交的相关文件的关键搜索文字 eg: 如TvApiHooker（不带后缀）
> + 连接自己的服务器 eg 10.126.69.28
> + 进入项目根目录，打开.repo--> manifests-->mac7p-atv-scbc.xml文件。
> + 搜索TvApiHooker关键字，查找对应的文件路径即可
> 5. 方法二
> + 找到Excel 中的最一栏，以http://10.126.16.60/ 开头的路径，这个是git提交路径 
> + 把开找到的链接，在信息行中，找到tree 文字，然后行点tree 进入。
> + 找到这笔提交的相关文件的关键搜索文字 eg: 如TvApiHooker.cpp
> + 连接自己的服务器 eg 10.126.69.28
> + rtk 文件一般在/kernel/android/R/vendor/realtek/common/ATV 下，先cd到目录
> + 输入命令搜索文件名 find -name "TvApiHooker.cpp" 
> + 搜索到会显示对应的路径eg./frameworks/native/ExtTv/src/TvApiHooker/TvApiHooker.cpp。

***
### 编译单个项目
***

> 1. 确定分支，然后同步代码 repo sync -j8 
> 2. 进入项目R文件夹,eg：2851M/kernel/android/R
> 3. 执行脚本 source build/envsetup.sh
> 5. 执行 lunch 找到对应的项目序号 
> 6. 2851M/2841A 序号为3 所以我们一般输入lunch 3 
> 7. 进入单个项目目录，eg:kernel/android/R/vendor/realtek/common/ATV/app/RtkTvProvider
> + 中间件TVMidwareManager路径为2851M/kernel/android/R/vendor/tv051/app/rtk_app/TVMidwareManager目录下
> 8. 替换需要编译的项目
> 9. 执行编译 mm -j32
> + 如果报错：build/make/core/Makefile:49: error: overriding commands for target `out/target/product/R4/system_ext/framework/tv-midware-manager.jar', previously defined at build/make/core/base_rules.mk:492，则用#号注释掉device/tv051/R4 或R3下的scbc.mk文件中的vendor/tv051/app/prebuilt_app/tvmidware/tv-midware-manager.jar:/system_ext/framework/tv-midware-manager.jar再编译，全编软件的时候记得恢复。

> 10. 编译完成后输出 build completed successfully （时间）
> 11. 完成后输出文件输出路径为 kernel\android\R\out\target\product\R4\system_ext\framework
> + 中间件输出文件为该目录下的tv-midware-manager.jar文件


***
### 中间件更新步骤
***

> 1. svn下载[中间件路径 ](https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/TVMidware/debug/[2851M])
> 2. 下载以上最新tv-midware-manager.jar文件push 到TV上 /system_ext/framework目录
> 3. 下载以上最新TVMidwareService.apk文件push 到TV上 /system_ext/app/TVMidwareService 目录

***
### 本地项目打包apptvmidware.jar步骤
***
> 1. 打开SVN的中间件项目，更新最新代码
> 2. 在android studio 工具上左下角的Buiild Variants : apptvmidware 选择 debug
> 3. 选中apptvmidware项目->Build ->make module
> 4. 右上角Gradlle->apptvmidware项目 ->Tasks->Other->makeJar->会在build->lib下生成apptvmidware.jar包 


***
### patch文件代码合并命令
***

> 1. 选择打开patch 文件，找到要patch 文件路径 Subject: &#91;PATCH&#93;
> 2. 连接自己的服务器 eg 10.126.69.28
> 3. 把patch文件放到该目录下
> 5. 执行patch -p1 < ？ ,？ 表示patch 文件全名，输入前面几个可tab 出来
> + eg: patch -p1 <0001.patch
> 6. 如果显示 Hunk #2 succeeded at 149 (offset 5 lines). 说明有冲突 
> + 出现冲突，会多出一个.orig的文件。eg :AndroidManifest.xml.orig （orig 为之前的文件）
> + git diff 查看冲突文件修改信息，（或者用对比工具比较）进行修改
> + eg :git diff /home/lifei/2851M/kernel/android/R/vendor/realtek/common/ATV/app/RtkTvProvider/AndroidManifest.xml
> + 对比是不是patch 要修改的内容，进行修改，然后删除 rm -rf AndroidManifest.xml.orig

     
***
### diff 文件代码合并命令方法
***

> 1. 选择打开diff 文件，找到要diff 文件路径 diff -&#45;git &#91;PATCH&#93;
> 2. 在自己的服务器上cd 到该目录下，把diff 文件放到该目录下
> 3. 执行git apply 文件路径(可以tab 出来)
> 4. 出现冲突 git diff 查看冲突文件修改信息，（或者用对比工具比较）进行修改


***
### bin 文件合并方法
***

> 1. 把.bin 文件放到U盘根目下
> 2. reboot -> 按住空格键 ，或者按住Esc键进入bootcode ，当显示Realtek> 可松开 
> 3. 输入 usb start
> 4. fatload usb 0:1 0x108000 ? , ?表示文件名 e:fatload usb 0:1 0x108000 vmlinux.bin
> 5. 输入 go all

***
### RTK代码sync code Excel提交同步分支相关路径
***
> 1. vendor_realtek_common_ATV_frameworks_native_ExtTv        
> + 2851M/kernel/android/R/vendor/realtek/common/ATV/frameworks/native/ExtTv
> 2. vendor_tv051_app           								 
> + 2851M/kernel/android/R/vendor/tv051/app
> 3. kernel_linux_linux-4.14_drivers_rtk_kdriver  			 
> + 2851M/kernel/linux/linux-4.14/drivers/rtk_kdriver 
> 4. ExtTv.java
> + kernel\android\R\vendor\tv051\app\exttv-framework\core\java\com\exttv\tv

***
### 查看DailyBuild 编译code 对应的版本
***
> 1. 打开编译DailyBuild 文件夹 eg:DailyBuild_RT2851M_0428
> 2. 打开save_apk_svn
> 3. 搜索自己要找的apk名
> 4. 找到Last Changed Rev: 14784
> 5. 14784 即为SVN 代码打包的代码
> 6. 如果自己编译代码想要理更改apk上的svn 版本，可修改服务器上的kernel\android\R\vendor\tv051\app\prebuilt_app\save_apk_svn的路径svn版本号，然后再编译。


***

## -----------------------------------TV相关----------------------------  


***
### 申请TV系统文件读写权限
***

> 1. adb connect 192.168.1.128
> 2. adb root
> 3. adb remount
> 4. 如果提示要重启则启以后，再走一次前三步一般情况即有文件读写权限，如果没有则按以下的申请
> 5. 开机 长按住ESC键进bootcode，显示Realtek>后可松开 
> 6. env set OEMLock off
> 7. env save
> 8. reset
> 9. 打开用adb工具
> 10. adb connect 192.168.1.128 （windowns :adb connect 192.168.1.128:5555 ）
> 11. adb root
> 12. adb disable-verity
> 13. adb reboot
> 14. adb connect 192.168.1.128
> 15. adb root
> 16. adb remount
> 17. 走完以上步骤即则可以对系统文件读写了


***
### 命令启动Activity界面
***

|  名称   | 命令  |
|  ----  | ----  |
| 工厂菜单  | am start -n com.apps.factory.ui/.designmenu.DesignMenuActivity |
| 设置界面  | am start -n com.apps.tvsettings/com.apps.tvsettings.ui.MainSettings|
| 信源菜单  | am start -n  com.apps.livetv/.SelectInputActivity|
| ATV搜台  | am start -n com.apps.atv/.atvsetup.AnalogueSetupActivity |
| RTK ATV  |am start -n com.realtek.tv.atv/.atvsetup.AnalogueSetupActivity |
| DTV搜台  |am start -n com.apps.dtv/.DigitalSetup.DigitalChannelSetupActivity |
| RTK DTV | am start -n com.realtek.dtv/.DigitalSetup.DigitalChannelSetupActivity|


***
### 遥控器显示工厂菜单方法
***

> 1. 设置主界面->Picture & Display -> Picture Adjustment ->Advanced Settings->Brightness -> 选中Contrast ->按数字1950/9735（design menu/factory menu）
> 2. 设置主界面->Picture & Display -> Picture Adjustment ->Advanced Settings->Brightness -> 选中Contrast ->连按OK五下



***
### CTS 版本文件挂载查看权限
***

> 1. 开机按住tab 键，或者按住Esc键进入bootcode，当显示Realtek> 后输入go r
> 2. 创建想要挂载对应的文件夹，想挂载tclconfig则 :mkdir /mnt/vendor/tclconfig
> 3. 把系统文件挂载到新创建的文件夹中,eg:mount -t ext4 /dev/block/？ /mnt/vendor/tclconfig,这个？号根据机型不同可能名字不同,41A/51M 如下面表格
> 4. 如果想把文件复制出来，则插上U盘，如果不停有日志打印影响输入，通过执行ps 命令查看所有进程，找到/sbin/loader_m进程的PID，执行kill PID 进程，eg: kill 109 
> 5. 执行 cp RMCA_ready /mmnt/udisk/sda1/ 把文件拷贝到U盘，文件夹则执行 cp -rf * /mmnt/udisk/sda1/
> 6. 拷贝完执行 sync 
> 7. 第6步不执行，有可能无法复制  
> 8. TV的挂载路径可以通过命令 ls -l dev/block/by-name/ 或者mount查询 ，如下表格    

2851M  

|  TV路径   | 对应挂载路径  |
|  ----  | ----  |
| /mnt/vendor/tclconfig  | mount -t ext4 /dev/block/mmcblk0p31 |
| /mnt/vendor/tvdata  | mount -t ext4 /dev/block/mmcblk0p33 |
| /mnt/vendor/impdata  | mount -t ext4 /dev/block/mmcblk0p34 |
| /mnt/vendor/factory  | mount -t ext4 /dev/block/mmcblk0p4 |
| /mnt/vendor/factory_ro  | mount -t ext4 /dev/block/mmcblk0p6 |

2841A   

|  TV路径   | 对应挂载路径  |
|  ----  | ----  |
| /mnt/vendor/tclconfig  | mount -t ext4 /dev/block/mmcblk0p29 |
| /mnt/vendor/impdata  | mount -t ext4 /dev/block/mmcblk0p31 |
| /mnt/vendor/factory  | mount -t ext4 /dev/block/mmcblk0p4 |
| /mnt/vendor/factory_ro  | mount -t ext4 /dev/block/mmcblk0p6 |
| /mnt/vendor/tvdata  | mount -t ext4 /dev/block/mmcblk0p30 |




***
### 反抄写
***
> 1. 把反抄写文件放到U盘根目录下
> 2. 上电一直按着ESC按住进bootcode，显示Realtek>后
> 3. 输入 mp_restore


***
### 预制频道通道查找
***
> 1. 用SQLiteStudio 打开DB
> 2. 打开SQL编辑器，查找 select type ,display_number, input_type from channels
> 3. input_type 即为通道，可以在中间件项目CommDefine 查找，或下表格。

|  通道值   | 通道  |
|  ----  | ----  |
| 0 | DTV_CABLE |
| 1  | DTV_ANTENA |
| 2  | DTV_SATELLITE  |
| 8 | ATV |
| 9 | ATV_CABLE |
| 10 |OTHERS |
| 11| ATSC_CABLE |
| 12| ATSC_ANTENA |
| 13| ISDB_CABLE |
| 14| ISDB_ANTENA |

***
### 查看用audio fw
***
> 1. 进kernel\system\configs文件夹
> 2. 找到对应的品牌配置
> 3. 打开找到AUDIO_ADDNAME ="_tv051_jbl" 后面就是用的aduio fw 名字的后缀
> 4. 进kernel\fw\audio_fw\4K文件夹 bluecore.audio.text_+ 上的后缀 ,即bluecore.audio.text_tv051_jbl.zip



***
### 其它相关操作
***


|  名称   | 具体操作  |
|  ----  | ----  |
| 系统升级 | 连接串口->reboot->长按tab等待日志出来 |
| 盲切ID  | 按遥控器0 6 2 5 9 8 MENU +ID    注：（ ID10 是010  ID 是三位数） |
| 键盘及遥控器切ID  | 062598 menu 047 前面这数字要用一排的那行数字输入，id 是三位数 |
| 串口切ID  | 选择中切ID行，input text ID |
| 改屏参 | 串口工具->断电上电->长按ESC(出现Realtek)->panel->选择序号->re(重启) |
| 屏蔽遥控器 | settings put global shop_ir_lock 0 |
| 设置tclconfig 权限 |mount -o remount,rw /mnt/vendor/tclconfig |
| U盘复制到TV当前路径| cp /mnt/media_rw/00AF-9C6B/RMCA_ATV/RMCA . |
| 删除文件| rm -rf R4（R4 表示文件名字） |
| 串口PASS 返回值| 固定值 AB 05 0A DF 4E （0A 为pass OE或OF 为Fail） |
| Log过滤关键字| logcat &#124; grep -E "word1 &#124; word2 &#124; word3" |
| 串口占用输出log| logcat -s  tag > data/log.log 或logcat -s FactoryUart > /data/temp/log.log |
| adb 保存log到文件| adb logcat > name.log |
| 工厂遥控器| 长按PAT 等上面两个灯长亮后，是RCA协议按000 ，NEC协议按001 ,松下按010 |
| 回退版本| git revert ID |



***
### 相关路径
***

|  名称   | 路径  |
|  ----  | ----  |
| 所有APP SVN总路径 | https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src |
| 工厂菜单SVN路径  | https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/Factory_General |
| 中间间SVN路径  | https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/TVMidware |
| 项目档案 | https://odm-design-center-hz.tclking.com/svn/Project_Document |
| 51M抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2851M安卓TV机芯项目档案 |
| 41A抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2841A机芯项目档案 |
| 41抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2841机芯项目档案 |
| 51抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2851机芯项目档案 |
| 红屏断言 |https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/LogApp |
| 新编译软件 |ftp://10.118.1.85/ProjectSoftware/TEST/MOKA-AMCS/ |
| 2851M代码管理| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632)|
| 问题测试表| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60701669](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60701669) |
| 应用&中间件 ReleaseNote| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=47625057](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=47625057) |
| 流程规范|[https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60696466](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60696466) |
| SPM版本编译发布releaseNote| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60693752](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60693752)|
| 软件批次订单| [https://rd-mokadisplay.tcl.com/rdm/](https://rd-mokadisplay.tcl.com/rdm/) |
| 2851M批次及发布软件时间| [https://docs.qq.com/sheet/DZUNJUWpWdVFVcWNO?tab=BB08J2](https://docs.qq.com/sheet/DZUNJUWpWdVFVcWNO?tab=BB08J2) |
| 2841A批次及发布软件时间| [https://docs.qq.com/sheet/DTEtSY3FVWnVjYWxv](https://docs.qq.com/sheet/DTEtSY3FVWnVjYWxv) |
| TV组每日任务| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=27963131](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=27963131) |
| TV组任务清单| [https://rd-mokadisplay.tcl.com/pms/secure/Dashboard.jspa#%E7%AD%9B%E9%80%89%E5%99%A8%E7%BB%93%E6%9E%9C/11224](https://rd-mokadisplay.tcl.com/pms/secure/Dashboard.jspa#%E7%AD%9B%E9%80%89%E5%99%A8%E7%BB%93%E6%9E%9C/11224)|



***
### git相关路径
***

|  名称   | 路径  |
|  ----  | ----  |
|  ExtTv源文件 | http://10.126.16.60/gerrit/gitweb?p=kernel/rtk_aosp/device/tv051/frameworks/native/ExtTv.git;a=tree;f=src/service;h=708692e5b70762932a775e7c10a913a8f0b16f33;hb=refs/heads/realtek/mac7p/android-11/scbc |
| 41/51串口路径  | http://10.126.16.60/gerrit/gitweb?p=kernel/rtk_aosp/device/tv051/app.git;a=blob;f=FactoryTools/src/java/com/realtek/factorytools/FactoryUart.java;h=a9686d993d4a6f1501f448d77570bef655c51a3a;hb=refs/heads/rt4151/mp210407_20Q4 |


***
### apk push 路径
***

|  apk 名称   | push路径  |
|  ----  | ----  |
| 工厂菜单apk | adb push /Users/tanlifei/Desktop/work/moka/project/Factory_General/app/release/Factory.apk /system_ext/app/Factory |
| 日志apk  | adb push /Users/tanlifei/Desktop/work/moka/project/LogApp/app/release/LogApp.apk /system_ext/app/LogApp |



***
### 共享目录
***
> 1. \10.126.69.28（2841A）
> 2. \10.118.1.100（2851M）


***
### 查看时间范围内的提交日志
***
> 1. git log --after="2022-3-1" --before="2022-5-1" --pretty=format:"%an %ad : %s" --date=short --no-merges --reverse
> + 参数	含义
> + –author	作者
> + –committer	提交者
> + –after	某时间后
> + –before	某时间前
> + –reverse	按时间顺序
> + –grep	提交说明包含字符串
> + -S	修改内容包含字符串
> + –pretty	格式化信息	oneline、short、full、fuller、format
> + –date	日期格式	relative、local、default、iso、rfc、short、raw
> + –no-merges	隐藏合并提交




***
### NPI项目
***

|  SPM   | 区域  | 项目名|
|  ----  | ----  | ----  |
| 李佳和 | 日规 | TVN21-A-023 日本I客户55A5000 1RT851E1ISA COST ST5461D12-6 TV项目 （日本安卓R真4K智能电视）|
| 龙飞扬 | 松下HDR10+ | TVN21-A-089 东欧松下75D8310L 1RT51MB1T2A CSOT ST7461D03-2 TV项目（台湾）|
| 龙飞扬 | 新西兰 | TVN21-C-067 东欧松下43D8300P 1RT51MC1S2B 配INX V430DJ1-Q01 D2 TV项目(新西兰）|
| 龙飞扬 | 泰霖 | TVN21-C-083 东欧松下50D8300P 1RT51MC1ISA配INX V500DJ7-QE1 (D5) TV项目（菲律宾））|
| 骆鑫 | CVT | TVP22-E-002 北非Stream客户43D6100F 5RT41AB1S2A HKC PT430CT02-4 TV派生项目 (销往欧洲 CVTE机芯）|
| 陈嘉平 | 阿根廷 | TVP21-L-009 南美Solnik客户 50D2090 1RT851A1ISA CHOT CV500U2-T01 V02 TV配屏项目（阿根廷）|
| 赵龙 | 战略P客户 | TVN22-C-023 战略P客户 32D2030 5RT41AB1S2A CSOT ST3151A07-2 V2.7 TV DVB-S2&T2项目（非洲TH-32LS670MF）|


## -----------------------常用账号-------------------

### Termius 账号
> * 1、Termius 账号179840045@email.top (版本7.34.1可重复用0045可以累加，密码随便)




***
### 预设频道路径
***
> 1. 确定客户的制式与工厂ID，提供db数据,必须有三个db文件和channel 文件夹
> 2. 进入自己服务/home/lifei/2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel目录下
> 3. 到factorydata_app对应目录下，找对应的工厂id 文件夹后，找对就的制式文件夹，进入dvb目录下，这三个文件夹没有则新建文件夹
> 4. 把tv.db 放到 rtk_provider_db还是provider_db目录下，具体放哪个文件，按以下确定
> + 区分rtk_provider_db目录还是provider_db 目录 ，把客户提供的DB 打，查看数据中的包名是否带com.realtek.（rtk），带则把db放到rtk_provider_db目录下，以com.apps则放provider_db 目录下
> 5. 打factorydata_vendor对应目录下，把channel下所有文件放到dvb/dtv_db目录下（如果dtv_db已经存在，有数据，可以不用在传，rtk_provider_db和provider_db可以共用一套）
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
    |63| 印尼Arisa工厂 |PGI|
    |64| 泰国Xtream工厂 |Xtream|