## 一：工具类库

##### 1：log4cpp

三个主要组件：日志类别category、输出源appender、布局layout

[log4cpp 的使用_log4cpp使用-CSDN博客](https://blog.csdn.net/mj348940862/article/details/127670078)

## 二：多媒体

音视频流媒体开发主要流程

音视频采集-编码-组包-网络编程tcp/udp-推流-流媒体转发-客户端拉流-socket通讯-解码-渲染

渲染包含：AR特效、贴纸、美颜等，技术(opencv、openGL)

##### 1：openCV

1：Mat类

由图像基本信息和图像数据data组成。一般的复制拷贝都是浅拷贝，data都是同一个，由引用计数跟踪。深拷贝使用函数

```text
Mat F = A.clone();
Mat G;
A.copyTo(G);
```

##### 2：ffmpeg

参考

https://ffmpeg.xianwaizhiyin.net/base-ffmpeg/ffmpeg-cmd-type.html

1：命令格式

ffmpeg+(input file para)+(-i input)+(output para)+(outfilename)+(global para)

```textile
outputPara:
-crf 23//constant rate factor码流控制参数，取值[0,51]，默认23,相差6，则码流相差
一倍。数值越小，图像越清晰，码流越大。
示列：ffmpeg -i <input> -c:v libx264 -crf 23 <output>
```

编码格式和封装格式转换

ffmpeg  -i test.mp4  -c:v libx264  -c:a copy   -vf format=yuv420p  test_h264.mkv  -y  -benchmark

2：推流和启动流媒体服务器

docker run --rm -it -e RTSP_PROTOCOLS=tcp -p 8554:8554 -p 1935:1935 -p 8888:8888 -p 8889:8889 aler9/rtsp-simple-server

推RTSP流

ffmpeg -re -stream_loop 100 -i  test.mp4 -c copy   -rtsp_transport tcp      -f rtsp rtsp://localhost:8554/mystream

输出的格式-f rtsp

推RTMP流

ffmpeg -re -stream_loop 100 -i test.mp4 -c:v libx264 -c:a aac  -f flv  rtmp://localhost/mystream

浏览器接收：  http://localhost:8888/mystream/

3：流媒体协议

| 协议     | 特点                                                                            | 延时  | 备注         |
| ------ | ----------------------------------------------------------------------------- | --- | ---------- |
| RTMP   | 视频必须h264，音频必须aac或mp3，多以flv封包，需flash player播放；现在多用来推流，因flash停止开发。只用TCP；兼容http； | 低   | 与http兼容    |
| RTSP   | 能自定义用TCP还是UDP；目前主要用在局域网内；底层是RTP和RTCP；                                         |     | 不直接与http兼容 |
| HLS    | 大多用来拉流，html5的播放器播放。HLS的视频编码不再流行，因此不用来推流了。                                     | 一般  | 浏览器主流      |
| webRTC | 底层用基于UDP的RTP；自适应码流；加密；NAT；不需要服务器和插件；不支持组播multicast技术，因此需要更多的带宽和资源来进行广播；       | 超低  | 可以p2p      |
| SRT    |                                                                               |     | 响应迅速       |
| DASH   | 和HLS类似，比之有更低的时延；                                                              |     | 浏览器主流      |

4：communication protocol

| http1     |     | s   |
| --------- | --- | --- |
| http2     |     |     |
| websocket |     |     |
| grpc      |     |     |
| webRTC    |     |     |

https://www.mirrorfly.com/blog/best-communication-protocols/

5：编码速度

preset={ultrafast、superfast、veryfast、faster、fast、medium、slow、slower、veryslow、placebo}

6：编码质量

profile

```textile
1:baseline profile//基本画质，支持I/P
2:extended profile//进阶画质，支持I/P/B/SP/SI帧
3:main profile//主流画质，自持I/P/B帧
4:high profile//高级画质
```

7：视觉优化参数

tune

```textile
tune的值有： film： 电影、真人类型；

animation： 动画；

grain： 需要保留大量的grain时用；

stillimage： 静态图像编码时使用；

psnr： 为提高psnr做了优化的参数；

ssim： 为提高ssim做了优化的参数；

fastdecode： 可以快速解码的参数；

zerolatency：零延迟，用在需要非常低的延迟的情况下，比如电视电话会议的编码。
```

8：

```c++
av_dict_set(&dictParam,"preset","medium",0);
av_dict_set(&dictParam,"tune","zerolatency",0);
av_dict_set(&dictParam,"profile","main",0);
avcodec_open2(pCodecCtx, pCodec,&dictParam);
```

##### 3：编码技术x264&x265

1：预测编码

    帧内预测

    帧间预测

2：变换编码

    从空间域变换到频域

3：熵编码  

    根据信号出现的概率来编码，类似哈弗曼编码

X264

0：问题

花屏：GOP有帧丢失。当发现GOP有帧丢失就丢弃这个GOP。

视频卡顿：接收下一个IDR有时延

1：GOP图像数据组包，一组包含10张左右图片，至少一张关键I帧，多张P前向参考帧，零张或多张B双向参考帧。P帧大小为I的一半，B帧为P帧的一半，B帧处理比较耗时。B帧的存在会导致时间戳同步较为困难。

2：IDR帧，就是GOP组的第一帧，是个特殊的I帧。IDR帧告诉解码器清空缓存区，解析新的GOP。

##### 4：openGL
