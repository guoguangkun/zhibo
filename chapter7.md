#第七章 
## web端推拉流的实现
---

“视频直播”是近两年互联网产业里很火的一个版块，大大小小的视频网站、APP层出不穷，而RTMP是目前市面上实现视频直播所采用的最主流的数据传输方式。常规的方式是，视频主播通过OBS等推流软件将摄像头捕捉的视频通过RTMP协议传输到指定的服务器地址，服务器将接收到的视频流以m3u8格式保存下来，客户端再通过拉取RTMP数据流的方式获取到视频数据并播放。

以上所描述大概就是一个基本的视频直播模型。那么，如果想要直接在浏览器中向RTMP服务器推流又该如何实现呢？

本章就来讨论在浏览器中向服务器推送rtmp视频流。