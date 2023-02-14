# linux

## <mark>linux命令</mark>

1:cut

cut  -d " "  -f1,2   tp.txt

## <mark>linux应用 </mark>

### 安装v2ray服务

bash <(curl -s -L https://raw.githubusercontent.com/xyz690/**v2ray**/master/go.sh)

bash <(curl -s -L https://git.io/v2ray.sh)

vmess选择websockt协议，别选择tcp，容易被公司内网搞混乱导致outbound fail

### 安装微信容器

sudo docker pull bestwu/wechat

docker run -d --name wechat --device /dev/snd --ipc=host  -v /tmp/.X11-unix:/tmp/.X11-unix  -v $HOME/WeChatFiles:/WeChatFiles  -e DISPLAY=unix$DISPLAY  -e XMODIFIERS=@im=fcitx  -e QT_IM_MODULE=fcitx  -e GTK_IM_MODULE=fcitx  -e AUDIO_GID=`getent group audio | cut -d: -f3`  -e GID=`id -g`  -e UID=`id -u`  bestwu/wechat

refrence:https://weisenhui.top/posts/54453.html

## <mark>linux系统</mark>

1：禁止自动更新：sudo apt remove unattended-upgrades，有可能在下次后面安装某个软件时，unattended-upgrades变成这个软件的依赖，并一起安装了，要注意。

配置禁止自动更新 sudo dpkg-reconfigure unattended-upgrades

### 内存信息查看

 https://blog.csdn.net/qq_36154886/article/details/108994911

## <mark>linux开发</mark>

1：编译opencv库时，发生所需要依赖库的位置冲突，系统库位置和ananconda里面库的位置。

​    方案：在系统变量PATH中去掉anaconda路径，cmake-gui清楚缓存后重新配置、生成 
