[[tools]]
一：搭建服务
1：安装
sudo apt-get install samba samba-common
2：添加用户
sudo smbpasswd -a sunyc
3：配置文件参数sudo vim /etc/samba/smb.conf
最后面添加
```
[sunyc]
comment = sunyc
browseable = yes
path = /home/sunyc/space2out #共享的目录
create mask = 0700
directory mask = 0700
valid users = sunyc
force user = sunyc
force group = sunyc
public = yes
available = yes
writable = yes
```
4：启动samba，默认445端口
sudo service smbd start
5：确认
netstat -lptn

二：window连接到samba
计算机->鼠标右击->映射网络驱动器->在对话框的文件夹填写：\\\\服务器ip