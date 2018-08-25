# git配置及使用

#### 密钥配置

1. git config --global  user.name "这里换上你的用户名"

2. git config --global user.email "这里换上你的邮箱"

3. ssh-keygen -t rsa -C "这里换上你的邮箱"

## 命令行 git
1. 拉取代码
``` git clone 仓库地址 
```
2. 切换分支
``` git checkout 分支名称
```
3. 查看状态
``` git status
```
4. 添加本地文件
``` git add .
```
5. 提交到本地
``` git commit -m "提交说明"
```
6. 推送到远程服务器
``` git push
```
7. 合并分支到master
在非master分支上开发完成后执行 ： 
``` 
git pull(切换到其他分支首先要pull)
git checkout master
git merge origin/develop
git push
```