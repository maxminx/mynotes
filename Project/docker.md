[[Area/tools|tools]]

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