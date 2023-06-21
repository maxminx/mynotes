[[Area/tools|tools]]

## 一：Git基本使用

##### 1：review config info

级别：系统级system，当前用户级global，当前仓库级local

git config --global --list

修改

```
git config --global user.name ``"myname"
git config --global user.email ``"test@gmail.com"
```

##### 2：Tag

查看：git tag

新建： git tag -a v0.0.1 -m "info"

删除：git tag -d tagname

更新远程：git push origin --tags

查看具体的信息：git show v2.1

##### 3：stash

git stash -a：暂存更改到本地，不记录git跟踪

查看：git stash list

取消暂存：git stash pop

##### 4：branch

查看：git branch -a/-r

新建：git checkout -b name

新建：git branch name

切换：git checkout name

删除：git branch -d name

快速切换到上一个分支：git checkout -

重移动重命名：git branch -M main

已经clone的前提下，再拉取某个特定的版本：

git checkout -b <本地分支名称>   origin/<远程分支名称>

git pull origin  远程分支名称

##### 5：update

git add .    跟踪改动过的文件

git add -u .    并对删除过的文件取消跟踪

##### 6：remote

git remote add origin **git**@**git**hub.com:maxminx/rockchip_npu.**git**    //建立本地虚origin仓库，指向绑定到远程真实git仓库

git push -uf origin main    //绑定远程虚仓库origin到本地真实分支main。相同作用**git** push --set-upstream origin main

直接下载关联：

git clone **git**@**git**hub.com:maxminx/rockchip_npu.**git**

git clone **ssh**://git@127.0.0.1:18022/sunyc/docs.git //已经绑定ssh公钥

##### 7：gitlab搭建

设置环境变量：**export** GITLAB_HOME=/srv/gitlab 

运行容器：

```bash
docker run --detach --hostname **gitlab**.example.com --publish 18443:443 --publish 18080:80 --publish 18022:22 --name **gitlab** --restart always --volume $GITLAB_HOME/config:/etc/**gitlab** --volume $GITLAB_HOME/logs:/var/log/**gitlab** --volume $GITLAB_HOME/data:/var/opt/**gitlab** --shm-size 256m gitlab**/**gitlab-ee:latest
```

