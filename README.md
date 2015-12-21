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

- 更新pom版本号、标记tag、提交+更新本次版本号并提交
 - mvn release:clean release:prepare -Dtag=1.6.0 -DdevelopmentVersion=1.7.0-SNAPSHOT -DreleaseVersion=1.6.0-RELEASE

- 解释-上述的执行后的两个commit操作：
 - prepare 1.6.0
 - prepare next release 1.7.0
```
min-xufpdeMacBook-Pro:j360-gitflow min_xu$ mvn release:clean release:prepare -Dtag=1.5.0 -DdevelopmentVersion=1.5.0-SNAPSHOT -DreleaseVersion=1.5.0-RELEASE
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] gitflow-version
[INFO] j360-module-version-autoupdate
[INFO] j360-module-version-autoupdate-dependment
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building gitflow-version 1.4.6-BUILD-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- maven-release-plugin:2.5.3:clean (default-cli) @ gitflow-version ---
[INFO] Cleaning up after release...
[INFO]
[INFO] --- maven-release-plugin:2.5.3:prepare (default-cli) @ gitflow-version ---
[INFO] Verifying that there are no local modifications...
[INFO]   ignoring changes on: **/pom.xml.backup, **/release.properties, **/pom.xml.branch, **/pom.xml.next, **/pom.xml.releaseBackup, **/pom.xml.tag
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git rev-parse --show-toplevel
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git status --porcelain .
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[WARNING] Ignoring unrecognized line: ?? release.properties
[WARNING] Ignoring unrecognized line: ?? target/
[INFO] Checking dependencies and plugins for snapshots ...
[INFO] Transforming 'gitflow-version'...
[INFO] Transforming 'j360-module-version-autoupdate'...
[INFO] Transforming 'j360-module-version-autoupdate-dependment'...
[INFO]   Updating j360-module-version-autoupdate to 1.5.0-RELEASE
[INFO] Not generating release POMs
[INFO] Executing goals 'clean verify'...
[WARNING] Maven will be executed in interactive mode, but no input stream has been configured for this MavenInvoker instance.
[INFO] [INFO] Scanning for projects...
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] Reactor Build Order:
[INFO] [INFO]
[INFO] [INFO] gitflow-version
[INFO] [INFO] j360-module-version-autoupdate
[INFO] [INFO] j360-module-version-autoupdate-dependment
[INFO] [INFO]
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] Building gitflow-version 1.5.0-RELEASE
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO]
[INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ gitflow-version ---
[INFO] [INFO] Deleting /Users/min_xu/Documents/IdeaProjects/j360-gitflow/target
[INFO] [INFO]
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] Building j360-module-version-autoupdate 1.5.0-RELEASE
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO]
[INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ j360-module-version-autoupdate ---
[INFO] [INFO]
[INFO] [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ j360-module-version-autoupdate ---
[INFO] [WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] [INFO] Copying 0 resource
[INFO] [INFO]
[INFO] [INFO] --- maven-compiler-plugin:2.3.2:compile (default-compile) @ j360-module-version-autoupdate ---
[INFO] [INFO] Nothing to compile - all classes are up to date
[INFO] [INFO]
[INFO] [INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ j360-module-version-autoupdate ---
[INFO] [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] [INFO] skip non existing resourceDirectory /Users/min_xu/Documents/IdeaProjects/j360-gitflow/j360-module-version-autoupdate/src/test/resources
[INFO] [INFO]
[INFO] [INFO] --- maven-compiler-plugin:2.3.2:testCompile (default-testCompile) @ j360-module-version-autoupdate ---
[INFO] [INFO] Nothing to compile - all classes are up to date
[INFO] [INFO]
[INFO] [INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ j360-module-version-autoupdate ---
[INFO] [INFO] No tests to run.
[INFO] [INFO]
[INFO] [INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ j360-module-version-autoupdate ---
[INFO] [INFO] Building jar: /Users/min_xu/Documents/IdeaProjects/j360-gitflow/j360-module-version-autoupdate/target/j360-module-version-autoupdate-1.5.0-RELEASE.jar
[INFO] [INFO]
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] Building j360-module-version-autoupdate-dependment 1.5.0-RELEASE
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO]
[INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ j360-module-version-autoupdate-dependment ---
[INFO] [INFO]
[INFO] [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ j360-module-version-autoupdate-dependment ---
[INFO] [WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] [INFO] Copying 0 resource
[INFO] [INFO]
[INFO] [INFO] --- maven-compiler-plugin:2.3.2:compile (default-compile) @ j360-module-version-autoupdate-dependment ---
[INFO] [INFO] Nothing to compile - all classes are up to date
[INFO] [INFO]
[INFO] [INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ j360-module-version-autoupdate-dependment ---
[INFO] [WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] [INFO] skip non existing resourceDirectory /Users/min_xu/Documents/IdeaProjects/j360-gitflow/j360-module-version-autoupdate-dependment/src/test/resources
[INFO] [INFO]
[INFO] [INFO] --- maven-compiler-plugin:2.3.2:testCompile (default-testCompile) @ j360-module-version-autoupdate-dependment ---
[INFO] [INFO] Nothing to compile - all classes are up to date
[INFO] [INFO]
[INFO] [INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ j360-module-version-autoupdate-dependment ---
[INFO] [INFO] No tests to run.
[INFO] [INFO]
[INFO] [INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ j360-module-version-autoupdate-dependment ---
[INFO] [INFO] Building jar: /Users/min_xu/Documents/IdeaProjects/j360-gitflow/j360-module-version-autoupdate-dependment/target/j360-module-version-autoupdate-dependment-1.5.0-RELEASE.jar
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] Reactor Summary:
[INFO] [INFO]
[INFO] [INFO] gitflow-version .................................... SUCCESS [  0.198 s]
[INFO] [INFO] j360-module-version-autoupdate ..................... SUCCESS [  1.125 s]
[INFO] [INFO] j360-module-version-autoupdate-dependment .......... SUCCESS [  0.029 s]
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] BUILD SUCCESS
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] [INFO] Total time: 1.481 s
[INFO] [INFO] Finished at: 2015-12-21T14:04:11+08:00
[INFO] [INFO] Final Memory: 10M/222M
[INFO] [INFO] ------------------------------------------------------------------------
[INFO] Checking in modified POMs...
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git add -- pom.xml j360-module-version-autoupdate/pom.xml j360-module-version-autoupdate-dependment/pom.xml
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git rev-parse --show-toplevel
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git status --porcelain .
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate-dependment/pom.xml.releaseBackup
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate-dependment/target/
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate/pom.xml.releaseBackup
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate/target/
[WARNING] Ignoring unrecognized line: ?? pom.xml.releaseBackup
[WARNING] Ignoring unrecognized line: ?? release.properties
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git commit --verbose -F /var/folders/57/zpw40ldx2kzdtwwm4kf9477c0000gn/T/maven-scm-45555875.commit pom.xml j360-module-version-autoupdate/pom.xml j360-module-version-autoupdate-dependment/pom.xml
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git symbolic-ref HEAD
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git push https://github.com/xuminwlt/j360-gitflow.git refs/heads/master:refs/heads/master
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Tagging release with the label 1.5.0...
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git tag -F /var/folders/57/zpw40ldx2kzdtwwm4kf9477c0000gn/T/maven-scm-1521173361.commit 1.5.0
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git push https://github.com/xuminwlt/j360-gitflow.git refs/tags/1.5.0
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git ls-files
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Transforming 'gitflow-version'...
[INFO] Transforming 'j360-module-version-autoupdate'...
[INFO] Transforming 'j360-module-version-autoupdate-dependment'...
[INFO]   Updating j360-module-version-autoupdate to 1.5.0-SNAPSHOT
[INFO] Not removing release POMs
[INFO] Checking in modified POMs...
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git add -- pom.xml j360-module-version-autoupdate/pom.xml j360-module-version-autoupdate-dependment/pom.xml
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git rev-parse --show-toplevel
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git status --porcelain .
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate-dependment/pom.xml.releaseBackup
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate-dependment/target/
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate/pom.xml.releaseBackup
[WARNING] Ignoring unrecognized line: ?? j360-module-version-autoupdate/target/
[WARNING] Ignoring unrecognized line: ?? pom.xml.releaseBackup
[WARNING] Ignoring unrecognized line: ?? release.properties
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git commit --verbose -F /var/folders/57/zpw40ldx2kzdtwwm4kf9477c0000gn/T/maven-scm-1398842325.commit pom.xml j360-module-version-autoupdate/pom.xml j360-module-version-autoupdate-dependment/pom.xml
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git symbolic-ref HEAD
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Executing: /bin/sh -c cd /Users/min_xu/Documents/IdeaProjects/j360-gitflow && git push https://github.com/xuminwlt/j360-gitflow.git refs/heads/master:refs/heads/master
[INFO] Working directory: /Users/min_xu/Documents/IdeaProjects/j360-gitflow
[INFO] Release preparation complete.
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] gitflow-version .................................... SUCCESS [ 23.842 s]
[INFO] j360-module-version-autoupdate ..................... SKIPPED
[INFO] j360-module-version-autoupdate-dependment .......... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------

```
