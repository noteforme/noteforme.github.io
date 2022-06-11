---
title: PerformanceMemory
date: 2022-06-07 10:34:22
tags: Performance
---





<img src="PerformanceMemory/2022-06-06-3.40.43.png" alt="2022-06-06-3.40.43" style="zoom:50%;" />





https://developer.android.com/studio/profile/memory-profiler?hl=zh-cn





查看内存限制，下面的命令不起作用

```
adb shell cat/system/build.prop // 需要root权限，没用

adb shell getprop  // 可以看全部的
adb shell getprop dalvik.vm.heapsize //迷你裙可以看到
```



####  内存抖动分析

Android profile

https://www.bilibili.com/video/BV1n44y1K7xS?p=5&spm_id_from=pageDriver

https://www.bilibili.com/video/BV1oz4y1m7Gw?p=6

https://www.youtube.com/watch?v=7ls28uGMBEs&list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE&index=73



Android 内存泄漏

https://www.bilibili.com/video/BV1n44y1K7xS?p=6

video 25分钟泄漏分析



leakcanary

https://github.com/square/leakcanary









AndroidStudio Profiler工具

[这个视频](https://www.youtube.com/watch?v=qJ1QLgiL24o) 讲解了找出内存泄漏的步骤　
还有AndroidStudio3.0的[图文方法](https://www.jianshu.com/p/bdfd2a6b2681) 





https://mp.weixin.qq.com/s/b_lFfL1mDrNVKj_VAcA2ZA



抖音内存监控框架 (kenzo)



#### 内存优化

操作1:25分钟

https://www.bilibili.com/video/BV1T44y1u7g6?p=4&spm_id_from=pageDriver



![20220608101926](PerformanceMemory/20220608101926.jpg)



https://juejin.cn/post/6844904099998089230#heading-6



https://blog.csdn.net/JArchie520/article/details/106418561
