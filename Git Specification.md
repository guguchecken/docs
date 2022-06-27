# 1. Git规范

## 1.1 分支规范

#### 1. 分支图



#### 2. 分支构成

##### 公司仓库分支构成

- master 分支：主分支，长期存在。
- release分支：内部发布分支，每两周发布一次。公司内部在此分支上进行开发。开发完成后将本分支内容cherry pick回master分支。
- matrixone分支：发布分支，每两个月发布一次。在经历四个内部发布之后创建，用于正式版本的发布。

#### 3. 分支命名

- matrixone 分支：一个大版本对应一个matrixone分支。
- release分支：一个大版本对应一个hotfix分支。例如hotfix_v0.1.0

## 1.2 issue 规范

## 1.3 release 规范

## 1.4 commit 规范

## 1.5 代码 review

## 1.6 PR 规范

#### 1. 标题规范

#### 2. label 规范

#### 3. PR commit 规范

开发时操作

1. 在Github上fork公司主仓库到个人仓库

2. 从个人仓库取回到本地

   ~~~bash
   #初始化一个空的本地仓库
   git init
   
   #设置个人姓名和邮箱
   git config user.name "test"
   git config user.email "test@matrixorigin.cn"
   
   #设置远程仓库
   git remote add origin git@github.com:your_repository/matrixone.git
   
   #设置上游仓库，在提交PR以及下次开发的时候使用
   git remote add upstream git@github.com:matrixorigin/matrixone.git
   
   #创建当前版本dev本地分支
   git checkout -b dev_v0.1.0
   
   #拉取远程仓库代码
   git pull origin dev_v0.1.0
   
   #创建本次开发对应的分支，这里以feature为例,其对应的 issue 为 #1
   git checkout -b feature_v0.1.0_#1
   
   #进行对应的开发
   
   #切换回对应版本dev分支
   git checkout dev_v0.1.0
   
   #使用squash merge将feature分支合并进dev分支
   git merge --squash feature_v0.1.0_#1
   
   #如果有冲突，则需要解决冲突
   #git add .
   
   git commit
   #按照标准填写merge commit信息
   
   #推送到远程仓库
   git push -u origin dev_v0.1.0
   
   ~~~

3. 在Github界面提起PR操作，并按照规范填写PR内容



