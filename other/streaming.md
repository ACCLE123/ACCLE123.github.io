### streaming media

需要完成一个摄像头流媒体服务器的搭建

<img src="./images/streaming media0.png"  style="zoom:70%;" />

---

#### server

##### 转发
1. 将摄像头拉流
2. 从本地进行流推送


##### 处理

1. 在本地服务器进行yolov处理
2. 推送处理的流


##### 触发

满足条件时需要触发一个函数


##### 存储

将(处理后的)流进行存储

---

#### tools

##### [ruoyi](https://github.com/yangzongzhuan/RuoYi)

在ruoyi上进行二次开发

##### [ZLMediaKit](https://github.com/ZLMediaKit/ZLMediaKit)

流媒体服务器框架

##### [ffmpeg](https://github.com/FFmpeg/FFmpeg)

流处理和转换工具
提供了ffplay作为流打开工具

##### [yolov](https://github.com/ultralytics/yolov5)

对视频流进行处理

##### [flask](https://github.com/pallets/flask)

搭建简单的python web服务器

---

#### arch

##### 单个摄像头架构
<img src="./images/arch0.png"  style="zoom:70%;" />

##### 多个摄像头架构
<img src="./images/arch1.png"  style="zoom:70%;" />

---

#### concept

##### web proxy

* rtsp: 应用层，基于tcp，用于控制播放、暂停、定位
* rtp: 应用层，基于udp，用于传输流媒体数据
* rtcp: 应用层，rtp的控制协议，用来确保流媒体数据的传输质量
* websocket: 基于tcp，全双工通行
  > http适合传输文本，并且是请求响应式，websocket可以传输二进制或者文本

---

#### other

##### bash script
编写start.sh stop.sh


##### encapsulation
python类的封装


##### thread
卡顿可以用threadpool优化得非常好
