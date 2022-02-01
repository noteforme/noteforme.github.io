---
title: VideoAudio
date: 2017-07-14 16:50:09
tags: 
categories: ANDROID

---
先安装codelabs把整个流程走一便，对库的使用有了整体的认识，
https://codelabs.developers.google.com/codelabs/exoplayer-intro/#1

 接着了解基本概念



语音转化

AudioApplication

> /Applications/VLC.app/Contents/MacOS/VLC --demux=rawaud --rawaud-channels 1 --rawaud-samplerate 44100 /Users/john/Desktop/audio/recording.pcm



#### 声音

##### 声音三要素

音调 : 音频的快慢  男生 < 女生 < 儿童

音量: 振动的幅度

音色:  谐波



##### PCM

采样大小 ： 一个采样用多少bit存放。常用16bit

采样率 	:  采样频率 8k , 16k , 32k, 44.1k 48k

声道数	： 单声道 ， 双声道 ， 多声道



##### WAV

![](/Users/john/Documents/noteforme.github.io/source/_posts/VideoAudio/Screen Shot 2021-03-07 at 5.21.18 PM.png)

![](/Users/john/Documents/noteforme.github.io/source/_posts/VideoAudio/Screen Shot 2021-03-07 at 5.23.23 PM.png)



#### 采集

##### 命令采集

```
ffmpeg -f avfoundation -i :0 out.wav  //采集
ffplay out.wav			//播放
```



