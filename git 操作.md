# git配置及使用   ----   [ 基础的用法 ] （更高级的百度）
> 说明： Git是一个扥不是的代码管理容器，本地和远端都有一份相同的代码。
> 构成： 本地代码、缓存区、提交历史

#### 1. git下载安装， 傻瓜式安装，【 [下载地址](https://www.git-scm.com/download/) 】

#### 2. 检查版本
> ` dos: git --version `


#### 密钥配置

- ```javascript
  git config --global  user.name "这里换上你的用户名"
```


- ```javascript
 git config --global user.email "这里换上你的邮箱"
```


- ```javascript
 ssh-keygen -t rsa -C "这里换上你的邮箱"
```

#### 命令行 git [ [相关说明](https://www.cnblogs.com/tocy/p/git-command-line-manual.html) ]

>  - 初始化本地git环境
```Javascript
 	git init
```
>  - 拉取代码
```Javascript
 	git clone 仓库地址
```
>  - 切换分支
```Javascript
	git checkout 分支名称
```
>  - 查看状态
``` Javascript
	git status
```
>  - 添加本地文件
``` Javascript
	git add .
```
>  - 提交到本地
``` Javascript
	git commit -m "提交说明"
```
>  - 推送到远程服务器
``` Javascript
	git push
```
>  - 合并分支到master,在非master分支上开发完成后执行 ：
```Javascript
	git pull(切换到其他分支首先要pull)
	git checkout master
	git merge origin/develop
	git push
```

##### 【 ★ [常用命令](https://www.cnblogs.com/allanli/p/git_commands.html) ★ 】