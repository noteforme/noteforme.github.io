---
title: location
date: 2022-05-24 14:48:58
tags:
---



https://www.youtube.com/watch?v=xTVeFJZQ28c

https://github.com/baurine/android-location-study/blob/master/note/location-note.md



https://blog.csdn.net/dxiaolai/article/details/81588921

https://blog.csdn.net/qq_32770809/article/details/115914021

https://blog.csdn.net/u013334392/article/details/91971758



其实在上面我已经提到了，所有上面的解决的方案都没有解决根本问题，那就是当你在室内开发时，你的手机根本就没法获取位置信息，你叫系统如何将位置信息通知给你的程序。所以要从根本上解决这个问题，就要解决位置信息获取问题。刚刚也提到了，只有NETWORK_PROVIDER这种模式才是室内定位可靠的方式，只不过由于大陆的怪怪网络，且大部分厂商也不会用google的服务，这种定位方式默认是没法用的。那怎么办？好办，找个替代的服务商就可以了，百度的位置信息sdk就可以解决这个问题。它的基本原理在上面已经提到过了，就是搜集你的wifi节点信息和你的手机基站信息来定位。

test


————————————————
版权声明：本文为CSDN博主「Sandy林」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u013334392/article/details/91971758
