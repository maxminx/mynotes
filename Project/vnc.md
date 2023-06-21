[[tools]]

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

