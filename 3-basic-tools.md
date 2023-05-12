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

## 二：docker

docker介绍

1：基本操作

免sudo权限

```bash
sudo usermod -aG docker $USER #添加当前用户到docker组
systemctl restart docker #重启docker服务
重启打开shell会话
```

其他

```bash
docker exec -it containerID bash
docker attach containerID#进入之前的终端

docker export containerName > img.tar
docker import img.tar newImageName

#force to remove
docker rm -f container
docker image rm -f img-name

#rename for image
docker image tag imgID  newImgName:version
#save container to a new image
docker commit -m="info" -a="author" containerID  newImageName:tag

#check resource consume
docker top containerID
docker stats

#加大共享内存--shm-size 8G
docker run -it --restart=always  --name=pysyc1.7  --gpus all --shm-size 16G  pytorch1.7-syc bash
```

据说在 Linux Docker中无法使用 systemd(systemctl) 相关命令的原因是 1号进程不是 init ，而是其他例如 
/bin/bash ，所以导致缺少相关文件无法运行。（System has not been booted with systemd as 
init system (PID 1). Can't operat）  
解决方案：/sbin/init  
如果提示没有/sbin/init，要先在容器中安装apt install -y init，再打包成新镜像docker export，再导入启动  
例如：Ubuntu 18.04 ，  
docker run -tid --name test --privileged=true ubuntu:18.04 /sbin/init  
docker exec -it test /bin/bash

2：docker网络

host模式：与宿主机共用网络，网络性能好

container模型：和另外的容器共用网络

None模式：容器关闭网络功能，安全性高、不会被攻击

Bridge模式：docker默认模式，虚拟docker0网关，通过Nat宿主与容器通信

n：安装微信

```bash
sudo docker pull bestwu/wechat

docker run -d --name wechat --device /dev/snd --ipc=host -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME/WeChatFiles:/WeChatFiles -e DISPLAY=unix$DISPLAY -e XMODIFIERS=@im=fcitx -e QT_IM_MODULE=fcitx -e GTK_IM_MODULE=fcitx -e AUDIO_GID=`getent group audio | cut -d: -f3` -e GID=`id -g` -e UID=`id -u` bestwu/wechat
```

详细的教程-杨文远  
https://www.kuangstudy.com/bbs/1439163376210096129

## 三：局域网远程VNC

##### 1：网页vnc—kasmVNC

```bash
#debian系安装
sudo apt install  ./kasmvncserver_*.deb#安装库
sudo addgroup $USER ssl-cert#加入ssl-cert组


#使用
kasmvncserver#启动服务
vncpasswd -u username -w -r#增加vnc会话用户

#查看会话
vncserver -list
#删除会话为：2的ID会话
vncserver -kill :2
```
