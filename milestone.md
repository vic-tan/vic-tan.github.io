---
layout: page title: "笔记"
description: "谭乐言"
header-img: "img/zhihu.jpg"
---

## 代码提交与合并

### RKT开发完成需同步到主分支(以2851M为例)

```dart
 * 1.到自己服服器
 * 2.cd进入2851M/kernel/android/R/vendor 目录下,如果是其它可以按下面几个步骤
    * 2-1.如果提供sync code Excel表格，则先找path路径关键词 到git(http://10.126.16.60/gerrit/#/admin/projects/)搜索到项目路径，
    * 2-2.找到对应的提交路径点 (gitweb)进入相对的文件目录-> 在heads 找到分支 点击shortlog
    * 2-3.找到对应的提交代码点击 复制commit id
 * 3.git log
 * 4.查看需要同步提交的记录log (如果分支不对，先切换分支)（或者直接在git 搜索到的）
    * 4-1.git checkout scbc/realtek/mac7p/android-11/scbc
    * 4-2.git log 是否有上面提交的记录
    * 4-3.git pull scbc realtek/mac7p/android-11/scbc
    * 4-4.git log --oneline 查看是否有上面提交记录
    * 4-5.git show 7e520f1d  查看修改内容并修改commit id(其它情况按上面步骤获得id
 * 5.切回到主分支 git checkout scbc/rt51m/master
 * 6.git log 确定分支切成功没有
 * 7.合并修改的内容 git cherry-pick commit id  报错说明有冲突，自己手动改再提交
 * 8.git push scbc HEAD:rt51m/master。
```

### 频道 DB 添加并同步到mastar 分支

注意：首先要确定当前分支，先从开发分支

```Ruby
* 1.到2851M/kernel/android/R/device/tv051/R4/custom_images/tclconfig/preset_channel目录下
* 2.到factorydata_app对应目录下，把tv.db 放到dvb目录下，如果是RTK的TV则是放到  rtk_provider_db目录下，否则放到provider_db目录下
* 3.到factorydata_vendor对应目录下，把channel下所有文件放到dvb/dtv_db目录下
* 4.到/2851M/kernel/android/R/device/tv051/R4/custom_imagesls
        * 5.git log 查看一下当前目录及当前日志
        * 6.git pull scbc realtek/merlin5/android-11/scbc 更新最新项目
        * 7.git status 查看是否有冲突
        * 8.git checkout ../..scbc.mk 有则回退
        * 9. git add  tclconfig/preset_channel/factorydata_app/2/ 添加文件夹
        * 10.git status 查看是否添加
        * 11.git commit -m "add rtk pl db tv" 添加提交信息
        * 12.git push scbc HEAD:realtek/merlin5/android-11/scbc 提交到分支
        * 13.git status 查看是否已提交
        * 14.复制刚才提交的commit id
        * 15.git checkout scbc/rt51m/master 切换到量产master 分支
        * 16.git log 查看当前分支信息及日志信息
        * 17.git pull scbc rt51m/master 同步当前分支
        * 18.git status
        * 19.git cherry-pick 2da8dd7f0f6c89042d4ee3eccacd503c33832bd5 同步开发分支commit id 文件
        * 20.git status 确定是否同步成功

```

