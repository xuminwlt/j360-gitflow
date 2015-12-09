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
