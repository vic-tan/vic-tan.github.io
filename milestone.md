---
layout: page title: "笔记"
description: "谭乐言"
header-img: "img/zhihu.jpg"
---

## 代码提交与合并

### RKT开发完成需同步到主分支(以2851M为例)


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



