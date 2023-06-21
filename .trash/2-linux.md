# linux

## Arch&Manjaro包管理

```bash
yay#syn and update
yay typora#search package and show availables
yay -Syu typora#install typora
yay -Sc#clean cache
yay -Rs#remove package and dependences
```

linux命令

## <mark>linux命令</mark>

1:cut

cut  -d " "  -f1,2   tp.txt

2:basename

3：sync强制把内存的缓冲区buffer更新到磁盘

## 视频相关命令

1：从摄像头获取一帧图片

v4l2-ctl  --set-fmt-video=width=320,height=240,pixelformat=YUYV   --stream-mmap --stream-count=1   --stream-to=grab-320x240-yuyv.raw

2：把yuv转换成png图片

ffmpeg -s 320x240 -pix_fmt yuyv422 -i grab-320x240-yuyv.raw grab.png

查看系统资源

1:vmstat

vmstat n m//采样m次，每次采样花n秒

## <mark>linux应用 </mark>

### 安装v2ray服务

bash <(curl -s -L https://raw.githubusercontent.com/xyz690/**v2ray**/master/go.sh)

bash <(curl -s -L https://git.io/v2ray.sh)

vmess选择websockt协议，别选择tcp，容易被公司内网搞混乱导致outbound fail

###refrence:https://weisenhui.top/posts/54453.html

## <mark>linux系统</mark>

1：禁止自动更新：sudo apt remove unattended-upgrades，有可能在下次后面安装某个软件时，unattended-upgrades变成这个软件的依赖，并一起安装了，要注意。

配置禁止自动更新 sudo dpkg-reconfigure unattended-upgrades

### 内存信息查看

 https://blog.csdn.net/qq_36154886/article/details/108994911

## <mark>linux开发</mark>

1：编译opencv库时，发生所需要依赖库的位置冲突，系统库位置和ananconda里面库的位置。

​    方案：在系统变量PATH中去掉anaconda路径，cmake-gui清楚缓存后重新配置、生成 

## <mark>Linux内核</mark>

驱动开发入门

子系统v4l2

[linux V4L2子系统——v4l2架构（1）之整体架构_楓潇潇的博客-CSDN博客

[V4l2框架分析_v4l2框架流程_welljrj的博客-CSDN博客](https://blog.csdn.net/welljrj/article/details/105578727?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-105578727-blog-114996084.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-105578727-blog-114996084.pc_relevant_default&utm_relevant_index=5)](https://blog.csdn.net/u013836909/article/details/125359789)
