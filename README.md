j360-gitflow
====
工作相关git使用指南

##分支##
- master
- release
- dev


##参与部门##
- 开发人员
- 测试人员
- 运维人员


##完整流程##
- 当前环境master，单一分支，创建dev分支
 - git checkout -b dev
- 提交新代码到dev，push到远程dev分支
 - git commit -m "some message
 - git push orgina dev
- 完成功能，fork dev分支到发布分支，版本号为0.0.1
 - git checkout -b release-0.0.1
 - 【循环节点】如果出现bug，开发人员checkout 该release版本，进行修复，再提交，并merge到dev分支

- 测试完成，merge到master
  - git branch master
  - git merge release-0.0.1

- 打包master
 - 生成tag
  - git tag -a 0.0.1 -m "0.0.1版本发布" master
  - git push --tags
 - 打包无误，删除release-0.0.1



##特殊说明##
 - release版本测试出bug
  - git checkout release-0.0.1
  - 此处开发人员可以签出分支，修复好bug后，再提交到remote仓库，供测试编译
 - master版本上线后出现bug，hotfix或者issue
  - git checkout -b issue-#001 master
  -  Fix the bug

  - git checkout master
  - git merge issue-#001
  - git push

 - 就像发布分支，维护分支中新加这些重要修改需要包含到develop分支中，所以小红要执行一个合并操作。然后就可以安全地删除这个分支了：
  - git checkout develop
  - git merge issue-#001
  - git push
  - git branch -d issue-#001

##和maven及配置文件的匹配##
 - maven
  - 开发新功能在dev分支pom中标注 0.0.2-snapshot
  - fork dev到release-0.0.2分支后，标注pom为0.0.2-build-snapshot
  - 切到master merge release-0.0.2，标注tag 0.0.2并在pom中标记未0.0.2-release，提交push
 - 配置文件(文件名仅供参考)
  - dev分支使用dev.properties
  - release分支使用test.properties
  - master分支使用production.properties

##使用maven-release-plugin发布版本号##
- 更新pom版本号
 - mvn --batch-mode release:update-versions -DautoVersionSubmodules=true -DdevelopmentVersion=1.3.0-SNAPSHOT

- 更新pom版本号发布并提交
 - mvn release:clean release:prepare -Dtag=1.3.0 -DdevelopmentVersion=3.8-SNAPSHOT -DreleaseVersion=3.9