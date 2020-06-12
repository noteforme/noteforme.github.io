---
title: Bluetooth04
comments: true
date: 2020-04-26 10:46:23
tags:
categories: BLE
---



##### 蓝牙问题汇总

https://race604.com/android-ble-in-action/

1. 连接断开之后可以根据实际情况进行重连，但如果是连接失败的情况，建议不要立即重连，而是调用`void closeBluetoothGatt()`清空一下状态，并延迟一段时间等待复位，否则会把gatt阻塞，导致手机不重启蓝牙就再也无法连接任何设备的严重情况。

  https://github.com/Jasonchenlijian/FastBle/blob/master/README_1.2.x.md



2. 每当设备断开连接时，请确保在BluetoothGatt对象上调用close()并将其设置为null,

   解决方法:始终在断开连接时关闭gatt实例，并在每次连接时创建一个新的gatt实例。

   在`onLeScan(..)`中启动一个新线程，然后进行连接

   https://stackoom.com/question/1Cyqr/Android-%E8%93%9D%E7%89%99%E4%BD%8E%E5%8A%9F%E8%80%97%E4%B8%8D%E7%A8%B3%E5%AE%9A

BluetoothLeScanner: could not find callback wrapper



3. 连接失败处理

   分两个平台来说，iOS端也有连接失败的委托，但是好像几乎不会发生这种情况，至少我从来没遇见过，而对于同款设备，android常常会出现连接失败的情况，`status != BluetoothGatt.GATT_SUCCESS`  ，android端开发请不要把连接失败和断开连接放在一块处理，因为断开连接可以直接尝试重新连接，而连接失败后尝试重新连接，需要加一些延时，并且需要gatt.close，清空一下状态，否则会把gatt阻塞导致手机不重启蓝牙就再也无法连接任何设备的情况。

http://liuyanwei.jumppo.com/2017/01/23/zhihu-live-a-hour-for-bluetooth-0.html

4. 扫描广播包

所有外设，只有在发出广播包的情况下，才能被central发现，绝大多数情况下，外设被连接后就不会发出广播（也有例外），很多人遇到无法找到设备的问题，大多属于这种情况。 重复扫描问题——————

http://liuyanwei.jumppo.com/2017/01/23/zhihu-live-a-hour-for-bluetooth-0.html



5. Android 7+ will stop scanning if it continues without stopping at all  for 30 minutes or more. This was added as a feature to prevent battery  drain in case developers had inadvertently or abusively left scanning on forever.

   https://github.com/AltBeacon/android-beacon-library/issues/528



https://blog.csdn.net/m0_37796683/article/details/83657204



6. status 133

   https://blog.csdn.net/baidu_26352053/article/details/54571688

7. unregisterApp() - mClientIf=5

   
   
8.  filter

   https://juejin.im/post/5d37d4d6f265da1bc414958a

9. 防止Android7中的BLE扫描滥用，从而做了一些限制，即不要在30s内对蓝牙扫描
   重复开启-关闭超过5次。

   https://blog.classycode.com/undocumented-android-7-ble-behavior-changes-d1a9bd87d983

   

### 应用如何做自动重连

其实自动重连比想象的要简单许多，无论是android还是ios端，只需要在设备断开连接的委托方法中，重新调用gatt.connet或者是centralManager.connet方法就可以了，无论当时设备是否有点，是否在周围，当设备再次开会或者连接到可连接范围内，都会自动被连上，就是这么简单。

http://liuyanwei.jumppo.com/2017/01/23/zhihu-live-a-hour-for-bluetooth-0.html





Intent mIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
startActivityForResult(mIntent, 1);



```
mBluetoothAdapter.enable();
mBluetoothAdapter.disable();
```





 https://blog.csdn.net/u014418171/article/details/81219297