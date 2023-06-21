[[hardware]]

##### 一：rgb相机sensors
CIS(CMOS image sensor)

1：Bayer filter，通过R、G、B像点激活sensors，下一步骤debayer通过demosaic插值形成rbg图片

2：RYYB filter模型，通过品红，黄、青色像点激活sensors。Bayer成像方式导致每个像点损失$\frac{2}{3}$的光能量

3：cmos sensors的卷帘曝光rolling shutter——基于逐行读取方式，对应高速物体有卷帘效益和车灯光流效应
	缺点：高速物体有拖影；曝光时间下限很高，通常为毫秒级别；
			
	
引用：
1： https://zhuanlan.zhihu.com/p/469199228
2： https://www.zhihu.com/people/liu-si-zhu-82 camera专家