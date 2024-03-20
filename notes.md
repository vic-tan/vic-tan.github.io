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
> 2. 命令进入需要编译的项目， eg: cd 2851m
> 3. 确保项目环境干净，防止出现因之前切换过分支导致环境不干净，先清除环境，输入如下清除命令 repo forall -c "pwd && git clean -xfd && git checkout -- ."
> 4. 确定要编译的分支，然后init编译的分支,可以在以下查找分支
> + [2841A分支管理链接](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35542944)
> + [2851M分支管理链接](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35523632)
> + 输入repo init -u patch 命令 eg:repo init -u ssh://10.126.16.60:29418/rt51M_manifest -m odin-gms.xml -b realtek/merlin5/android-11/scbc
> 5. 同步当前分支代码 命令:repo sync -j8
> 6. 同步过程如果提示报错文件，可以先删除该文件，然后重新从2步开始，还是不行可以直接删除R3或R4文件，再重新第2步开始
> + 删除文件命令 eg:rm -rf kernel/android/R/vendor/R4
> 7. 执行脚本 ./std-build.sh 如果是Google tv 则执行std-build_GTV.sh  回车
> + 文件一定是在项目根目录下执行，在根目下，可以打前几个然后tab 出来 
> + 此过程会自动去下载关联apk，如果此时想编译自己本的apk，工具日志下载完apk后就可可以直接替换自己的apk到对应的目录下，这样编译出来的就自己的apk。apk 路径一般在kernel/android/R/vendor/tv051/app/prebuilt_app/目录下
> 8. 请输入版本号[Please type version] 按回国为默认V8-T851MGL-LF1V001版本号，最好按时间命名加上自己的标签，ge:V8-T851MGL-LF1V20220421_TAN
> 9. CTS or not?[1-CTS 2-Debug 3-CTS&Debug] 表示编译软件类型，一般输入 2
> + 1-CTS，表示用户模式，有很多权限限制
> + 2-Debug，表示开发模式，有root等多个权限，适合开发
> 10. Build OTA package or not? 表示是否支持OTA升级，一般输入 n
> 11. 是否启用远场语音[n/y]? 表示当前软件是否支持远场语音，一般输入 n，41A硬件不支持这项
> 12. Clean or not? 是否先清除下环境，如果走了第3步，一般选n
> 13. 是否编译中间件，一般选y, 如果中间件不影响可以选n
> 13. 是否编译RTK公版? y表示编译 RTK UI ,n 表示moka UI
> 13. 编译标志位，一般我们选0
> 14. 请输入客户品牌[Please type product][0-NOKIA(Default) 1-MOTOROLA 2-NOKIA_2K 3-PANASONIC_EU 4-PANASONIC_FFM 5-PANASONIC_BASE 6-Amati 7-KALLEY_FFM 8-KALLEY_BASE] 品牌选择（1，2 表示区域客户，3、4表示松下）
> 14. 默认没有数字标牌，是否集成数字标牌 [n/y] ，这个一般选 n 
> 15. 回车后，如果是清空环境第一次编译，一般要等待2个小时，已经编译过第二次，则基本上30分钟。
> 16. 编译完成后会输出 Image creation complete. Output file:install_wipe.img 为正常
> 17. 编译完成后会输出路径为根目录 eg:2851M/Target/V8-T851MGL-LF1V20220421_TAN  



***
### 编译单个项目
***

> 1. 确定分支，然后同步代码 repo sync -j8 
> 2. 进入项目R文件夹,eg：2851m/kernel/android/R
> 3. 执行脚本 source build/envsetup.sh
> 5. 执行 lunch 找到对应的项目序号 
> 6. 2851M/2841A 序号为3 所以我们一般输入lunch 3 
> 7. 进入单个项目目录，eg:kernel/android/R/vendor/realtek/common/ATV/app/RtkTvProvider
> + 中间件TVMidwareManager路径为2851m/kernel/android/R/vendor/tv051/app/rtk_app/TVMidwareManager目录下
> 8. 替换需要编译的项目
> 9. 执行编译 mm
> + 如果报错1：build/make/core/Makefile:49: error: overriding commands for target `out/target/product/R4/system_ext/framework/tv-midware-manager.jar', previously defined at build/make/core/base_rules.mk:492，则用#号注释掉device/tv051/R4 或R3下的scbc.mk文件中的vendor/tv051/app/prebuilt_app/tvmidware/tv-midware-manager.jar:/system_ext/framework/tv-midware-manager.jar再编译，全编软件的时候记得恢复。
> + 如果报错2： error: overriding commands for target `out/target/product/R4_GTV/system_ext/framework/tv-midware-manager.jar', previously defined at build/make/core/base_rules.mk:492
11:23:25 ckati failed with: exit status 1，则用#号注释掉/kernel/android/R/device/tv051/R4_GTV 下的scbc.mk文件中的vendor/tv051/app/prebuilt_app/tvmidware/tv-midware-manager.jar:/system_ext/framework/tv-midware-manager.jar再编译，全编软件的时候记得恢复。 如果整编也会报这个错，则把2851m/kernel/android/R/vendor/tv051/app/rtk_app/TVMidwareManager 删除，或者把2851m/kernel/android/R/vendor/tv051/app/rtk_app/TVMidwareManager/Android.mk 文件改个名字即可。

> 10. 编译完成后输出 build completed successfully （时间）
> 11. 完成后输出文件输出路径为 kernel\android\R\out\target\product\R4_GTV\system_ext\framework
> + 中间件输出文件为该目录下的tv-midware-manager.jar文件


***
### 编译kernal
***
> 1. 确定分支，然后同步代码 repo sync -j8 
> 2. 进入项目R文件夹,eg：2851M/kernel/android/R
> 3. 执行脚本 source build/envsetup.sh
> 5. 执行 lunch 找到对应的项目序号 
> 6. 2851M/2841A  序号为9 所以我们一般输入lunch 9 
> 7. cd kernal/system
> 8. ./build_android.sh -p NOKIA.cfg -c n -v userdebug -j 8 -K y
> + 如果是日规，则把上面的NOKIA改成SHARP.cfg

***
### 中间件更新步骤
***

> 1. svn下载[中间件路径 ](https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/TVMidware/debug/[2851M])
> 2. 下载以上最新tv-midware-manager.jar文件push 到TV上 /system_ext/framework目录
> 3. 下载以上最新TVMidwareService.apk文件push 到TV上 /system_ext/app/TVMidwareService 目录

***
### 本地项目打包apptvmidware.jar->TVMidwareService.apk 生成->TVMidwareManager.jar包步骤
***

> 1. 打开SVN的中间件项目，更新最新代码
> 2. 添加接口成功后不停的->Build ->make prjoject直到不报错，
> 3. 执行->Build ->rebuild project 会在apptvmidware项目中的build->outputs->aar中生成apptvmidware-debug.aar文件
> 4. 把TVMidwareManager/libs/tv/apptvmidware-debug.aar文件替换成上面刚生成的apptvmidware-debug.aar文件
> 4. 把TVMidwareManager 文件夹都复制到服务器，按单编流程编译出tv-midware-manager.jar文件

> 1. 打开SVN的中间件项目，更新最新代码
> 2. 在android studio 工具上左下角的Buiild Variants : tvmidware 和TVMidwareManager选择tv_rt2841aDebug, apptvmidware 选择 debug
> 3. 选中apptvmidware项目->Build ->make module
> 4. 右上角Gradlle->apptvmidware项目 ->Tasks->Other->makeJar->会在build->lib下生成apptvmidware.jar包 

> 1. 选中tvmidware项目->Build ->Genrate Signed Bundle or APK -> APK ->Module 选择MiddleCommon.tvmidware ,填写key和密码->选择tv_rt2841ADebug->Finish 
> 2. 会在tvmidware项目下->tv_rt2841A->debug->TVMidwareService.apk





***
### 中间件新增接口步骤
***

> 1. 在apptvmidware包下java.com.tv.tvmidwaremanager下对应的应用类中添加新接口，如：IFactory类，新接口最好加在类最后。
> 2. 在com.tv.tvmidwaremanager.app 目录下找到与第一步对应的Service.aidl添加对应接口，如IFacotryService.aidl类。
> 3. 在tvmidware 中对应的Impl实现接口，如:FactoryAIDLImpl 
> 4. 在TVMidwareManager 中对应的AdapterManager和AdapterProxy 实现接口
> 5. 也可在选中apptvmidware 然后选项Build-> Make module 未实现的接口会自动报错，然后去实现即可

***
### 编译 exttv-framework.jar步骤
***

> 1. cd RT2851M\kernel\android\R
> 2. 执行source build/envsetup.sh
> 3. 执行 lunch 9
> 4. 进到exttv路径：RT2851M\kernel\android\R\vendor\tv051\app\exttv-framework\core
> 5. 执行 mm
> 6. 生成的jar包路径：RT2851M\kernel\android\R\out\target\common\obj\JAVA_LIBRARIES\exttv-framework_intermediates
> 7. 注意是classes.jar文件，然后更改名字为exttv-framework.jar 替换中间件TVMidwareManager下rt2841A 下的exttv-framework.jar

***
### 编译 factory-adapter-tv051.jar步骤
***

> 1. cd RT2851M\kernel\android\R
> 2. 执行source build/envsetup.sh
> 3. 执行 lunch 9
> 4. 进到exttv路径：RT2851M/kernel/android/R/vendor/tv051/app/rtk_app/FactoryAdapter
> 5. 执行 mm
> 6. 生成的jar包路径：RT2851M\kernel\android\R\out\target\common\obj\JAVA_LIBRARIES\factory-adapter-tv051_intermediates/classes.jar
> 7. 注意是classes.jar文件，然后更改名字为factory-adapter-tv 替换中间件TVMidwareManager下rt2841A 下的factory-adapter-tv.jar


***
### 同步开发代码
***  

> 1. 连接进入自己的服务器 eg : 10.126.69.28
> 2. 命令进入需要编译的项目， eg: cd 2851M
> 3. 切换到开发分支,可执行repo init -u patch 切到对应分支，或者cd 到有仓库的文件目录下git checkout 切换分支
> 4. 同步当前分支代码 命令:repo sync -j10
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
> 15. 如果报错报黄说明有冲突，需要找到冲突文件，修改冲突文件内容，再提交 ，可以通过git diff 来查看冲突文件
> 15. 修改提交信息git commit -&#45;amend ——>control+x ——> Y回车
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
> 3. 在表格中找到路径栏，一般文件夹是以_，从左到右即为服务器中的路径
> 4. 如果在路径栏无法直接定位路径，可以按下面方法寻找
> 5. 方法一（推荐）
> + 找到Excel 中的最一栏，以http://10.126.16.60/ 开头的路径，这个是git提交路径 
> + 把开找到的链接，在信息行中，找到tree 文字，然后行点tree 进入。
> + 找到这笔提交的相关文件的关键搜索文字 eg: 如TvApiHooker（不带后缀）
> + 连接自己的服务器 eg 10.126.69.28
> + 进入项目根目录，打开.repo--> manifests-->mac7p-atv-scbc.xml文件。
> + 搜索TvApiHooker关键字，查找对应的文件路径即可
> 6. 方法二
> + 找到Excel 中的最一栏，以http://10.126.16.60/ 开头的路径，这个是git提交路径 
> + 把开找到的链接，在信息行中，找到tree 文字，然后行点tree 进入。
> + 找到这笔提交的相关文件的关键搜索文字 eg: 如TvApiHooker.cpp
> + 连接自己的服务器 eg 10.126.69.28
> + rtk 文件一般在/kernel/android/R/vendor/realtek/common/ATV 下，先cd到目录
> + 输入命令搜索文件名 find -name "TvApiHooker.cpp" 
> + 搜索到会显示对应的路径eg./frameworks/native/ExtTv/src/TvApiHooker/TvApiHooker.cpp。



***
### gerrit 找仓库查找
*** 

> 1. 先按【RTK 同步Code 代码的仓库查找】找到代码对应的路径
> 2. 然后进入路径执行git remote -v
> 3. 显示出来的以/kernel到最后面就是仓库路径
> 4. 打开 gerrit—> Projects->List->Filter->输入搜索仓库路径-> 找到仓库路径然后点击gitWeb-> 显示出的heads即是当前仓库的所有分支 


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
> + 每次改完最用git diff 查看是否跟给的patch 文件相同

     
***
### diff 文件代码合并命令方法
***

> 1. 选择打开diff 文件，找到要diff 文件路径 diff -&#45;git &#91;PATCH&#93;
> 2. 在自己的服务器上cd 到该目录下，把diff 文件放到该目录下
> 3. 执行git apply 文件路径(可以tab 出来),如果apply 不进去可以用patch -p1 < xx.diff 试试
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
### 夹版本后查看提交code版本
***
> 1. 先找夹到相邻的有差异的两个软件版本。
> 2. 找到异常两个版本后，找到Reports文件夹中的.xml 文件，eg:V8-T851MGL-0020445_2022060809.xml
> 3. 打开对比工具比较两个.xml文件，所差异文件即为两差异版本修改code
> 4. 根据对比差异中path路径及revision（commid id）进行相关的查找。也可以根据据RTK sync code Excel表格 找出对日期提交的code进行查看。
> 5. 如果要回退到两个相差的版本，可以将.xml 文件替换编译项目下的.repo文件中的manifest.xml
> 6. 注意，如果替换过了manifest.xml内容， 然后又做了repo forall -c "pwd && git clean -xfd && git checkout -- ." manifest.xml文件会重新替换，我们得又重新替换 )
> 7. 替换完后执行 repo sync 
> 8. 通过差异的.xml 进行code 回退，通过git checkout id （回退后恢复也是用这个）
> 9. 编译软件即可


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
| moka 设置界面  | am start -n com.apps.tvsettings/com.apps.tvsettings.ui.MainSettings|
| RTK 设置界面  | am start -n com.realtek.menu/com.realtek.menu.MainActivity|
| 信源菜单  | am start -n  com.apps.livetv/.SelectInputActivity|
| ATV搜台  | am start -n com.apps.atv/.atvsetup.AnalogueSetupActivity |
| RTK ATV  |am start -n com.realtek.tv.atv/.atvsetup.AnalogueSetupActivity |
| DTV搜台  |am start -n com.apps.dtv/.DigitalSetup.DigitalChannelSetupActivity |
| RTK DTV | am start -n com.realtek.dtv/.DigitalSetup.DigitalChannelSetupActivity|



***
### 新工厂遥控器P模式恢复老的协议
***
> 1. 因为新的工厂遥控器只能关P模式不能打开P模式，而老的工厂遥控器可关可开，用新的遥控器恢复老的遥控器协议方法
> + SET+EXIT 3秒进入设置模式（闪灯）-->P（闪灯）-->0-->OK（闪灯）-->2-->1（闪灯）-->2-->0-->0-->OK（闪灯）-->5-->SET（闪灯）-->4-->5-->SET（闪灯）-->6-->SET（闪灯）-->4-->5-->SET（闪灯）-->5-->SET（闪灯）-->6-->3-->SET

***
### 修改屏幕倒转
***

> 1. 找到倒屏ID 的model.ini 文件
> 2. 打开id model文件,找到【PANEL】 中的路径 如：m_pPanelName = "/mnt/vendor/tclconfig/panel/UHD_ST4251B05-2.ini";
> 3. 找到上面【PANEL】路径文件，因为是tclconfig 目录，所以必须用串口输入指令： su，然后再输入mount -o remount,rw /mnt/vendor/tclconfig 申请权限 
> 4. 把上面【PANEL】路径文件先导出来，才可以进行编辑。
> 5. 导出后打开文件，找到 VFLIP 字段，如果1则设置0，如果是0则设置为1，后保存
> 6. 把文件导入到TV上面【PANEL】路径文件，替换之前的文件
> 7. 打开工厂菜单，进入Service Menu ，切换一下其它ID ，然后在切回到倒屏ID，重启即生效（因为修改的文件不是真正生效文件，必须要切ID后才会把数据拷贝到生效文件中，所以必须要切一下ID） 


***
### 遥控器显示工厂菜单方法
***

> 1. 设置主界面->Picture & Display -> Picture Adjustment ->Advanced Settings->Brightness -> 选中Contrast ->按数字1950/9735（design menu/factory menu）
> 2. 设置主界面->Picture & Display -> Picture Adjustment ->Advanced Settings->Brightness -> 选中Contrast ->连按OK五下



***
### CTS 版本文件挂载查看权限
***

> 1. 开机按住tab 键，或者按住Esc键进入bootcode，当显示Realtek> 后输入go r
> 2. 创建想要挂载对应的文件夹，想挂载tclconfig则 :mkdir /mnt/vendor/impdata
> 3. 把系统文件挂载到新创建的文件夹中,eg:mount -t ext4 /dev/block/？ /mnt/vendor/impdata,这个？号根据机型不同可能名字不同,41A/51M 如下面表格
> 4. 如果想把文件复制出来，则插上U盘，如果不停有日志打印影响输入，通过执行ps 命令查看所有进程，找到/sbin/loader_m进程的PID，执行kill PID 进程，eg: kill 109 
> 5. 执行 cp RMCA_ready /mmnt/udisk/sda1/ 把文件拷贝到U盘，文件夹则执行 cp -rf * /mmnt/udisk/sda1/ 或者在要拷贝的上一级目录 cp -r impdata /mmnt/udisk/sda1/
> 6. 拷贝完执行 sync 
> 7. 第6步不执行，有可能无法复制  
> 8. TV的挂载路径可以通过命令 ls -l dev/block/by-name/ 或者mount查询 （正常开机后输此命令才可以查询，然后复现出来从第1步开始），如下表格    

> 9. 示例impdata
> + mkdir /mnt/vendor/impdata
> + mount -t ext4 /dev/block/mmcblk0p1 /mnt/vendor/impdata
> + ps
> + kill 110
> + cd mnt/vendor/impdata
> + cp -rf * /mmnt/udisk/sda1/

> 9. 示例factory
> + mkdir /mnt/vendor/factory
> + mount -t ext4 /dev/block/mmcblk0p4 /mnt/vendor/factory
> + ps
> + kill 110
> + cd mnt/vendor/factory
> + cp -rf * /mmnt/udisk/sda1/

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
### 克隆软件
***
> 1. 用NTFS格式的U盘，剩余空间需要大于16G。
> 2. 将usb插入电视。
> 3. 将串口工具连接到PC和电视，并确保串口打印正常。
> 4. 按住电脑键盘上的ESC键并保持静止，电视已关闭并打开，当您输入uboot时，它将显示Realtek>，然后您可以释放ESC按钮。
> 5. 输入命令“mp_dump”将数据从emmc克隆到U盘，当您看到复制进度条时，克隆已开始。
> 6. 生成大约需要20分钟。U盘的根目录中会生成mp_user.bin，mp_boot0.bin，mp_boot1.bin三个文件，大约16 GB。克隆完成后，串行端口将显示成功。
> 7. 压缩三箱文件并将其发送给我们。

***
### 反抄写（回写)
***
> 1. 将3个文件mp_user.bin，mp_boot0.bin，mp_boot1.bin拷贝到U盘
> 2. 连上串口工具，断电上电一直按着ESC按住进bootcode，显示Realtek>后
> 3. 串口工具输入命令mp_restore将镜像写入emmc
> 4. 大约需要20分钟左右
> 5. 恢复完成后，断电重启电视
> 6. 验证P模式，Uart Enable，预置的频点，音量，power mode，是否正确


***
### RTK extTv 方法调试(比如调试extledControl 方法)
***
> 1. 进入串口
> 2. su
> 3. setenforce 0
> 4. exttvtest 
> 5. 选27（facFunctionSet）
> 6. ledControl
> 7. 输入1或者2或者3，可以控制亮光颜色
> 8. 看结果




***
### 登录google 账号
***
> 1. adb shell input text weishengqin6@gmail.com
> 2. adb shell input text MOKA@123





***
### 查看用audio fw
***
> 1. 进kernel\system\configs文件夹
> 2. 找到对应的品牌配置
> 3. 打开找到AUDIO_ADDNAME ="_tv051_jbl" 后面就是用的aduio fw 名字的后缀
> 4. 进kernel\fw\audio_fw\4K文件夹 bluecore.audio.text_+ 上的后缀 ,即bluecore.audio.text_tv051_jbl.zip

***
### 色温标准值
***

|  类型   | x  | y  |
|  :----  | :----  | :----  |
|  Normal  | 283  | 297  |
|  Cool    | 267  | 277  |
|  Warm    | 313  | 329  |
|  Warm2   | 314  | 329  |


***
### A310色温仪常见问题
***
> 1. 报HDMI HIGH LIGHT C-A err 时需要校准下色温仪，把探头刻度对到0-CAL,然后放到黑色区，按色温仪CAL 黄色按钮，三行显示有000瞬间即可，然后把探头刻度对到MEAS开始使用
> 2. 报Warm register thanless MinRGB err 时,白平衡工具ofs项中LimitValue min 值不对，Normal 与 Cool &Warm 相加不参小于或大于 LimitValue值
> 3. 报TV switch to WB adjust err  时，有可能时当前ID 应该是RTK UI，但就为是Dailyduild 版本，显示的是Moka UI，导致无法跳转到HDMI1 ,可以查看日志跳转HDMI 指令是否正常。
> 4. 报Project ID code err 时，把白平衡工具SNO项中的P-ID check的勾去掉
> 5. 报MAC addr code or net err 时，把白平衡工具SNO项中的MACcheck的勾去掉
> 6. 报SW code err 时，把白平衡工具SNO项中的sw ver option 和ignore-chk的勾选
> 7. 报Device ID read err 指TV 没有抄Device ID key 用U盘抄一个key即可
> 8. 报 HDMI high light C-A err 表示色温仪未校验证
> 9. HDMI high light err 色温亮度不够，有可能是内置白场没打开
> 10. MAX lum check Brightness err 最大亮度调不过，有可能是内置白场没打开
> 11. ::WBA lum check Brightness err 内置白场没打开

***
### 22293仪常见问题
***
> 1. 打100%白场——>PATTERN——>按101（或者转动滚轮到显示为00101w:100%）——> 按ENTENR ——> 按OUT
> 2. 打100%白场——>PATTERN——>按103（或者转动滚轮到显示为00103w:80%）——> 按ENTENR ——> 按OUT


***
### 其它相关操作
***


|  名称   | 具体操作  |
|  ----  | ----  |
| 系统升级 | 连接串口->reboot->长按tab等待日志出来 |
| 盲切ID  | 按遥控器0 6 2 5 9 8 MENU +ID    注：（ ID10 是010  ID 是三位数） |

| 键盘及遥控器切ID  | 062598 menu 047 前面这数字要用一排的那行数字输入，id 是三位数 |
| 切ID  | 选择中切ID行，input text ID |
| 串口切ID  | projectidchanger->按enter 然后会弹出来所有的model ini文件，前面有数字，再输入数字就可以切到对应的project了|
| 串口输入按键  | input keyevent 106（键值） |
| 改屏参 | 串口工具->断电上电->长按ESC(出现Realtek)->panel->选择序号->re(重启) |
| 绑定遥控器 | 串口工具->断电上电->长按ESC(出现Realtek)->irda ->输入遥控器品牌序号（0为自动识别）-> irda_filter_disable->reset |
| 屏蔽遥控器 | settings put global shop_ir_lock 0 |
| 设置tclconfig 权限 |mount -o remount,rw /mnt/vendor/tclconfig |
| U盘复制到TV当前路径| cp /mnt/media_rw/00AF-9C6B/RMCA_ATV/RMCA /system_ext/app/ |
| 删除文件| rm -rf R4（R4 表示文件名字, 多个文件可以在后面加空格，然后接第二个文件路径） |
| 串口PASS 返回值| 固定值 AB 05 0A DF 4E （0A 为pass OE或OF 为Fail） |
| Log过滤关键字| logcat &#124; grep -E "word1 &#124; word2 &#124; word3" |
| 串口占用输出log| logcat -s  tag > data/log.log 或logcat -s FactoryUart > /data/temp/log.log |
| adb 保存log到文件| adb logcat > name.log |
| 工厂遥控器| 长按PAT 等上面两个灯长亮后，是RCA协议按000 ，NEC协议按001 ,松下按010 |
| 回退版本| git revert ID |
| 回退修改| git checkout . |
| cd 后回退上一次目录 | cd - |
| 查找当前git 仓库 | git remote -v |
| 杀app进程 | ps -All -> kill 【pid】 |
| 获取TV 所有栈activity| adb shell dumpsys activity activities |
| 获取TV 最上层activity | adb shell dumpsys activity top \| grep ACTIVITY \| grep mResumedActivity (windowns 把 grep 改为 findstr )|
| 不亮屏无法连接串口修改 | 串口工具->断电上电->长按ESC(出现Realtek)->help->找到urat0_enable enable—>输入urat0_enable->re|
| 工厂遥控器查看键值 | logcat -s KCR-KeyConverter\|logcat -s getevent|
| code 查找文件或关键字或多个条件 | find -name "TvApiHooker.cpp" \|grep -nr 关键字 \| (grep -nr isdb \| grep -nr TVAPP_TYPE\ =\ 1)|
| 串口设置Global | settings put global 字段 值|
| 串口设置prop属性 | setprop 字段 值|
| 串口获取prop属性 | getprop 字段 值 , getprop 是获取所有|
| 设置当前文件夹下所有文件权限 | chmod 777 . -R |
| 设置mac adb 快捷指令 | ~/.oh-my-zsh/plugins/git/|
| 抄key GTV 工具测试订单号 | TR_IDX188090A_RD    20230605114801|
| 抄key R+ 工具测试订单号 | TR_IDK197775B_RD    20230605114801|
| 抄key 2875P 工具测试订单号 | TR_DHR230539_RD   20231220164801|
| 串口控制台上输入 dumpsys tv_input | 查看LiveTV 信源信息（DTV/ATV/HDMI路径等）|
| 串口控制台上输入 settings list global | 查看所有全局保存Global.putInt数据 |
| 串口控制台上输入 settings list secure| 查看所有全局保存Secure.putInt数据 |
| 时时查看应用CPU 点用率| adb shell top |

***
### 相关路径
***

|  名称   | 路径  |
|  ----  | ----  |
| 所有APP SVN总路径 | https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src |
| 工厂菜单SVN路径  | https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/Factory_General |
| 中间件SVN路径  | https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/TVMidware |
| 项目档案 | https://odm-design-center-hz.tclking.com/svn/Project_Document |
| 51M抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2851M安卓TV机芯项目档案 |
| 41A抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2841A机芯项目档案 |
| 41抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2841机芯项目档案 |
| 51抄Key址址 | https://odm-design-center-hz.tclking.com/svn/Project_Document/RT2851机芯项目档案 |
| 红屏断言 |[https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/LogApp](https://odm-design-center-hz.tclking.com/svn/scbc_apps/trunk/app/src/LogApp) |
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
| 泰国OAD码流制作方法| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60715849](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60715849)|
| 日本OAD码流制作方法| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=55457911](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=55457911)|
| 工厂菜单调中间件接口| [https://rd-mokadisplay.tcl.com/kms/display/SWHelloWorld/090112-Factory](https://rd-mokadisplay.tcl.com/kms/display/SWHelloWorld/090112-Factory)|
| 工厂菜单调中间件接口说明| [https://rd-mokadisplay.tcl.com/kms/display/SWHelloWorld/11+Factory](https://rd-mokadisplay.tcl.com/kms/display/SWHelloWorld/11+Factory)|
| HDMI认证问题点收集| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=27980806](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=27980806)|
| Settings.Global.put() 保存路径 | /data/system/users/0/settings_global.xml|
| 中间件架构| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35540875](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=35540875)|
| 对外工厂生产流程文档| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=65295773](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=65295773)|
| Git提交规范文档| [https://rd-mokadisplay.tcl.com/kms/display/SWHelloWorld/02+Code+Review](https://rd-mokadisplay.tcl.com/kms/display/SWHelloWorld/02+Code+Review)|
| 频道频道表| [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=74159472](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=74159472)|
| 软件工厂支持 | [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=65295769](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=65295769)|
| 生产适应性 | [https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=55456670](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=55456670)|
| KMS-生产适应| [https://rd-mokadisplay.tcl.com/pms/browse/SWPD-611](https://rd-mokadisplay.tcl.com/pms/browse/SWPD-611)|
| KMS-Mantis重点问题 | [https://rd-mokadisplay.tcl.com/pms/browse/SWPD-194](https://rd-mokadisplay.tcl.com/pms/browse/SWPD-194)|
| KMS-生产问题及售后问题 | [https://rd-mokadisplay.tcl.com/pms/browse/SWPD-656](https://rd-mokadisplay.tcl.com/pms/browse/SWPD-656)|

***
### 修改机器屏参
***

> 1. su(root)
> 2. cd /mnt/vendor/tclconfig/panel/(你这个id-使用的屏参)
> 3. cp /mnt/vendor/tclconfig/panel/（查到屏参）.ini /mnt/vendor/factory/bin_panel/rtkPanel.ini
> + (55寸) cp /mnt/vendor/tclconfig/panel/UHD_ST5461D12-6.ini /mnt/vendor/factory/bin_panel/rtkPanel.ini
> 4. sync
> 5. reboot

***
### code 路径
***

|   名称   | code路径  |
|  ----  | ----  |
| ExtTv | kernel/android/R/vendor/tv051/app/exttv-framework/core/java/com/exttv/tv/ExtTv.java |
| app| kernel/android/R/vendor/realtek/common/ATV/app|
| rtk_app| kernel/android/R/vendor/tv051/app/rtk_app|
| RtkTvProvider| kernel/android/R/vendor/realtek/common/ATV/app/RtkTvProvider|
| RtkKeyInterceptServer | kernel/android/R/vendor/tv051/app/rtk_app/RtkKeyInterceptServer |
| FactoryAdapter| kernel/android/R/vendor/tv051/app/rtk_app/FactoryAdapter |
| FactoryTools| kernel/android/R/vendor/tv051/app/rtk_app/FactoryTools |
| 41/51串口| kernel/android/R/vendor/realtek/common/ATV/FactoryTools/src/java/com/realtek/factorytools/FactoryUart.java |
| frameworks| kernel/android/R/vendor/realtek/common/ATV/frameworks |


***
### toDosk
***

|名称 |设备代码 |  临时密码）|
| :----- | :---- | :---- |
| mac | 505 707 890 |w69nx49k |
| windowns home| 223 944 126 | 3u9uj986 |
| windowns pc| 724 657 086| xvw7iep1 |


***
### 全擦升级
***

> 1. 运行 qrtice-customer-version1-0.0.39-en
> 2. 选择kernel/fw/bootcode/RTD28XOB8_A1_TV051_R3_2K/bootcode_for_rtice.bin，选择Type 选择Burn Directly
> 3. 选择点update->上电（要等开机灯灭了才上电，有电容要放完电）
> 4. 选择kernel/fw/bootcode/RTD28XOB8_A1_TV051_R3_2K/dvrboot.rescue.exe.bin 放到U盘根目录 ->连拉串口工具按esc上电进入boot（Fat 32）
> 5. 发送 usb start;fatload usb 0:1 0x1500000  dvrboot.rescue.exe.bin;go 0x1500000
> 6. 升级img 软件


***
### 预制频道表查找与更新方法
***

> 1. 找到中间件com.tv.tvmidwaremanager.constant.TvContractEx 类，区分预制频道各种Input type 
> + TYPE_DTV_CABLE = 0;
> + TYPE_DTV_ANTENNA = 1;
> + TYPE_DTV_SATELLITE = 2;
> + TYPE_ATV = 8;
> + TYPE_ATV_CABLE = 9;
> + TYPE_OTHERS = 10;
> + TYPE_ATSC_CABLE = 11;
> + TYPE_ATSC_ANTENNA = 12;
> + TYPE_ISDB_CABLE = 13;
> + TYPE_ISDB_ANTENNA = 14;
> 2. 把/home/lifei/2851m-dev/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel/factorydata_app 目录下各工厂db 数据下载出来
> 3. 用SQLite 工具打开 ，打开SQL 编译器
> 4. SQL查询select input_type,package_name,type,display_number,display_name,internal_provider_flag1 from channels order by input_type
> 5. 根据input_type类型把display_number得到对应的表即可
> 6. input_type 即为通道，可以在中间件项目CommDefine 查找，或下表格。
> 7. 预制频道通道维护表[https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=74159472](https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=74159472)





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
### 常用TV ID 尺寸 ID
***

| 名称 | 方案 |  DVB | ATSC |  ISDB |  Colombia | DTMB |
| :----- | :---- | :---- | :---- | :---- | :---- |
| ATV（51M）55寸 | RTK UI  | 10(带卫星) | /     | 152   | /       | 607(香港)  |
| ATV（51M）55寸 | moka UI | 404       | 424   | 457   | 480     |  /        |
| ATV（41A）43寸 | RTK UI  | 137       | /     | /     | /       |  /        |
| ATV（41A）43寸 | moka UI | 122       | 250   | /     | /       |  /        |
| GTV（51M）43寸 | RTK  UI | 12        | /     | 137   | 209/112 |  /        |
| GTV（41A）43寸 | RTK  UI | 12        | /     | 137   | 209/112 |  /        |
| R+（）65寸     | RTK  UI | 13        | 30     | 3   | 18      |  /        |



***
### 制式（2841&2851机芯制式）
***

> 1. DVB(8M)（eg：惠州工厂DVB制式）OK
> + ATV ----ATV全，不用切signal type
> + Cable ---Cable下的DTV------------DVBC
> + Antena ---Antena下的DTV---------DVBT
> + Sat --- Sat下的DTV-----------------DVBS

> 2. colombia 中美哥伦比亚 (DVB 6M) OK
> + ATV ---有Cable ATV和Antena ATV 通过signal type切换
> + Antena ---只有DTV

> 3. ISDB（信源列表显示TV，通过signal type切换）（eg：惠州工厂isdb制式）OK
> + Cable ----只有ATV
> + Antena ---有ATV和DTV

> 4. ATSC（信源列表显示TV，通过signal type切换）(eg:RTK 泰林工厂ATSC制式数据)(OK)
> + Cable  ---有ATV和DTV
> + Antena ---有ATV和DTV

> 5. BTMD （信源列表显示ATV/DTV），不用切signal type
> + ATV ----ATV全，不用切signal type 
> + DTV ---Antena下的DTV---------DVBT


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

> + 提取某时间范围的所有仓库修改记录~比如要看2-25到2-28改了什么：
> + repo forall -c git log --since="2020-02-25" --until="2020-02-28" > ~/log.log
> + 提取某时间范围的所有仓库某个人的修改记录
> + repo forall -p -c git log --author="tanlifei@tcl.com" --since="2022-04-03 08:00:00" --until="2022-04-15 23:59:59" > ~/masterlog.log


***
### 查看git 仓库是否共仓库方法
***

> 1. 首页切到你要同步的分支
> 2. 进入到你要同步代码的文件路径
> 3. git remote -v 即可查到当前文件的在的仓库。如：kernel/rtk_aosp/device/realtek/app/RTKMenu，或者在gerrit回到git仓库后，找到页面URL ssh://lifei@10.126.16.60:29418后面到.git即仓库
> 4. 创建一个临时文件夹，然后初始化你要同步的仓库到临时文件夹 如：repo init -u ssh://10.126.16.60:29418/rt51M_manifest -m odin-gms.xml -b realtek/merlin5/android-11/scbc
> 5. 不需要下载代码，直接打开临时文件夹下的.repo/manifests/41a-51m-scbc-common.xml
> 6. 在文件中找到kernel/rtk_aosp/device/realtek/app/RTKMenu 仓库后，查看revision 即是它的共仓库 如：revision="rt41a/master_220718"，则说明你要同步的支与rt41a/master_220718是共仓的，你只要提交rt41a/master_220718即可，如果是相同，说明为独立仓库,
> 7. 另一种方法可以在[https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60697622]https://rd-mokadisplay.tcl.com/kms/pages/viewpage.action?pageId=60697622 中搜索仓库，如果没有表示共仓。 


***
### git 多笔CODE 同步
***

> + git cherry-pick xxx_id1..xxx_id3
> + 注意中间的两个点，表示把两个commit区间的所有commit多复制过去
> + 单个commit只需要git cherry-pick commitid
> + 多个commit 只需要git cherry-pick commitid1..commitid100
> + 注意，不包含第一个commitid ， 即 git cherry-pick (commitid1..commitid100]
> + 如果想搞成[]区间，使用 git cherry-pick A^..B 相当于[A B]包含A

***
### git commit后（尚未git push操作），需要回退的情况
***

> 1.  git cherry-pick xxx_id1..xxx_id3
> + 比如commit id信息为：90f1ce4d73c5dc63f46fa61984a6bb878f47374
> 2. 执行git reset -&#45;soft HEAD^操作
> + 对应HEAD即上述commit id信息 git reset -&#45;soft 90f1ce4d73c5dc63f46fa61984a6bb878f47374^
> + 说明：最后的符号^记得不要漏掉,此时通过git status时，可以看到git add 的文件(绿色）
> 3. git reset file_name 或者git reset . 用来回退全部
> + 通过git reset file_name 操作后，再git status可以看到红色的修改文件
> 4. git checkout file_name
> + 通过git checkout modify_file还原至修改之前状态


***
### git push 未reView 需要回退的情况
***

> 1. 先在git 上 abandon
> 2. git reset id (这个id 是回到退到哪一笔的id ,而不是你要回退的那一个),到这一步如果又想提交，可以重新add/commit/push 不需要再走下面的步骤了
> 3. git checkout -&#45; .
> 4. git log
> 5. git status
> 6. 如果找不回代码，可到gerrit上找到abandon 对应的记录，打开，找到DOWLOAD ,下载diff.zip
> 7. 把diff解压放到app 项目根目录下，然后在as 上的git 上输入patch -p1 <patch.diff 即可
> 8. 如果自己修改了想打成patch 可以用git diff > changes.patch 方法执行


***
### git 常见用法
***
> + git checkout -- . ：清除当前目录下所有未暂存的文件。
> + git clean -xfd ：清除当前目录下所有Untracked 的文件。



***
### R+ 断电上电串口不输出修改串口状态输出logcat
***
cd /mnt/vendor/factory/bin_panel 
sed -i s/BOOT_UART0_ENABLE\ 1/BOOT_UART0_ENABLE\ 0/g 000BootParam.h 
sed -i s/BOOT_UART0_ENABLE\ 1/BOOT_UART0_ENABLE\ 0/g 000BootParam.h_backup 

***
### Mac iTerm2 设置命令别名方法
***

> + 打开访达—> 前往文件夹->输入~/.oh-my-zsh/plugins/git —> git.plugin.zsh
> + 打开git.plugin.zsh 文件 
> + 在文件最后添加别名，eg: alias acon='adb connect 192.168.1.128'
> + 重启 iTerm 直接输入acon 即代表输入adb connect 192.168.1.128
> * 自定义别名列表
*|别名 |指令 |  
    | :----- | :---- |
    |act| adb connect 192.168.1.128|                            
    |acr| adb connect 192.168.1.128 && adb root && adb remount| 
    |art| adb root| 
    |arm| adb remount| 
    |adv| adb disable-verity| 
    |abt| adb reboot| 
    |asl| adb shell| 
    |mnc| minicom| 
    |alg| adb logcat > log001.log| 
    |aslg| adb logcat > log001.log| 
    |apf| adb push /Users/tanlifei/Desktop/work/moka/project/Factory_General/app/release/Factory.apk /system_ext/app/Factory| 
    |apl| adb push /Users/tanlifei/Desktop/work/moka/project/LogApp/app/release/LogApp.apk /system_ext/app/LogApp| 
    |gst| git status| 


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
    *|文件夹号 |工厂 |  代号（数据库名）|
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
    |65| 阿尔及利亚Stream system工厂 |STREAM|
    |66| 瓜多尔Audioelec工厂 |AUDIOELEC|
    |67| 阿尔及利亚Brandt工厂 |BRANDT|
    |68| 埃及 MTI 工厂 |MTI|
    |69| 埃及Fresh 工厂 |FRESH|


***
### 工厂遥控器
***

|键名 |功能 |  
| :----- | :---- |
    | VGA/Menu | 打开设置菜单| 
    | TV | DTV| 
    | Cable | ATV| 
    | Source | 打开信源列表 | 
    | CMP1 | 现在没用，以前是做YPbPr /亚马逊的3P项目用来触发wifi强度获取| 
    | 3D | 打开蓝牙列表 | 
    | OOB | 复位 | 
    | Menu | 在ATV/DTV/信源无效果，在HDMI/AV 下显示主菜单，moka UI 在左上角显示菜单，RTK UI 显示设置菜单 | 
    | RATLE | ATV 音量80 | 
    | C.TEMP | 调色温 | 
    | L.SR | 打开按键模式 | 
    | PW | 进老化/退老化 | 
    | P | 开P模式/关P模式 | 
    | USB | madioplay | 
    | SN | 进入/退出工厂售后界面 |
    | WIFI | 打开WIFI界面 |
    | SET | 调左右声道平衡-15 ,0 15 循环切 |


***
### 常用串口指令表
***

|别名 |指令 |  
| :----- | :---- |
    | 远场语音模块状态 | AA 07 97 28 00 5E 0D| 
    | 启动LED控制 | AA 07 FC 05 12 77 5E| 
    | LED控制状态查询 | AA 07 FC 05 13 67 7F| 
    | 喇叭 | AA 07 9F 1D 00 0D CC| 
    | 光纤 | AA 07 9F 1D 01 1D ED|
    | HDMI-ARC | AA 07 9F 1D 03 3D AF|  