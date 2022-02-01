---
title: Bluetooth03
comments: true
date: 2018-06-11 09:41:39
tags:
categories: BLE
---



####  经典蓝牙 低功耗蓝牙的区别

http://www.loverobots.cn/the-analysis-is-simple-compared-with-the-classic-bluetooth-and-bluetooth-low-energy-in-android.html

[蓝牙适配建议](https://juejin.im/post/59964187f265da249600af68)



*  蓝牙低功耗

  ​	Android 4.3 为发挥核心作用的[蓝牙低功耗](https://developer.android.com/guide/topics/connectivity/bluetooth-le.html?hl=zh-cn)（*蓝牙 LE*）引入了平台支持。在 Android 5.0 中，Android 设备现在可以发挥蓝牙 LE *外围设备*的作用。应用可以利用此功能让附近设备发现它。例如，您可以开发这样的应用：让设备发挥计步器或健康监测仪的作用，并与其他蓝牙 LE 设备进行数据通信。

  新增的 `android.bluetooth.le` API 让您的应用可以发布广告、扫描响应以及与附近的蓝牙 LE 设备建立连接。要使用新增的广告和扫描功能，请在您的清单中添加 `BLUETOOTH_ADMIN` 权限。当用户更新您的应用或从 Play 商店下载您的应用时，会被要求向您的应用授予以下权限：“Bluetooth connection information:Allows the app to control Bluetooth, including broadcasting to or getting information about nearby Bluetooth devices.”

  要启动蓝牙 LE 广播，以便其他设备能发现您的应用，请调用 `startAdvertising()`，并传入 `AdvertiseCallback` 类的实现。回调对象会收到广播操作成功或失败的报告。

  Android 5.0 引入了 `ScanFilter` 类，让您的应用可以只扫描其感兴趣的特定类型设备。要开始扫描蓝牙 LE 设备，请调用 `startScan()`，并传入筛选器列表。在方法调用中，您还必须提供 `ScanCallback` 的实现，以便在发现蓝牙 LE 广播时进行报告。

###  蓝牙基本操作

#### 蓝牙权限

```
 <uses-permission android:name="android.permission.BLUETOOTH" />
```

如果您希望您的应用启动设备发现或操作蓝牙设置，则还必须声明 `BLUETOOTH_ADMIN` 权限。 大多数应用需要此权限仅仅为了能够发现本地蓝牙设备。 除非该应用是将要应用户请求修改蓝牙设置的“超级管理员”，否则不应使用此权限所授予的其他能力。 **注**：如果要使用 `BLUETOOTH_ADMIN` 权限，则还必须拥有 `BLUETOOTH` 权限。 如打开关闭蓝牙

```
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
```

##### 6.0权限

```
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

#####  设置蓝牙

1. 判断设备是否支持

   [BluetoothAdapter](https://developer.android.com/reference/android/bluetooth/BluetoothAdapter.html?hl=zh-cn) 

   ```
   mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
   BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
   if (mBluetoothAdapter == null) {
       // Device does not support Bluetooth
   }
   ```

   

2. 启用蓝牙

   ```
    if (mBluetoothAdapter==null||!mBluetoothAdapter.isEnabled()){
           Toast.makeText(this,"设备不支持蓝牙权限",Toast.LENGTH_SHORT).show();
     }else {
           Toast.makeText(this,"支持设备",Toast.LENGTH_SHORT).show();
     }
   ```

#### 查找设备

5.0以下的方式就不贴出来了，代码里有

```
  /**
     * android5.0以上使用
     */
    @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
    private  void  scanDevices(){
        BluetoothLeScanner mBluetoothScanner = mBluetoothAdapter.getBluetoothLeScanner();
        mBluetoothScanner.startScan(scanCallback);
    }

    ScanCallback scanCallback = new ScanCallback() {
        @Override
        public void onScanResult(int callbackType, ScanResult result) {
            super.onScanResult(callbackType, result);
            //把byte数组转成16进制字符串，方便查看
            Log.e(TAG,"onScanResult :"+result.getScanRecord().toString());
            BluetoothDevice device = result.getDevice();
            blueAdapter.addDevice(device);
            
            ScanRecord scanRecord = result.getScanRecord();
            int rssi = result.getRssi();
        }

        @Override
        public void onBatchScanResults(List<ScanResult> results) {
            super.onBatchScanResults(results);
        }

        @Override
        public void onScanFailed(int errorCode) {
            super.onScanFailed(errorCode);
        }
    };

```



##### 连接设备

蓝牙设备经常处于关机状态，先调用下面方法

```
BluetoothDevice remoteDevice = adapter.getRemoteDevice(address);
```

 ```
remoteDevice.connectGatt(context, true, mGattCallback);//参数1：上下文。
                                                       //参数2：是否自动连接（当设备可以用时）
                                                       //参数3：连接回调。
 ```

https://www.cnblogs.com/Free-Thinker/p/11507349.html



**6、关于autoConnect参数为true的意义？**

在蓝牙核心文档Vol3: Core System Package[Host volume]->Part C: Generic  Access Profile的Connection Modes and Procedures章节中有涉及到自动连接建立规程(Auto  Connection Establishment Procedure)的定义。

自动连接建立规程用来向多个设备同时发起连接。一个中央设备的主机与多个外围设备绑定，只要它们开始广播，便立刻与其建立连接。跟多细节请参考蓝牙核心文档和协议栈源码。



回调

```
private BluetoothGattCallback mGattCallback = new BluetoothGattCallback() {
    /**
     * Callback indicating when GATT client has connected/disconnected to/from a remote GATT server
     * @param gatt     返回连接建立的gatt对象
     * @param status   返回的是此次gatt操作的结果，成功了返回0
     * @param newState 每次client连接或断开连接状态变化，STATE_CONNECTED 0，STATE_CONNECTING 1,STATE_DISCONNECTED 2,STATE_DISCONNECTING 3
     */
    @Override
    public void onConnectionStateChange(BluetoothGatt gatt, int status, int newState) {
        super.onConnectionStateChange(gatt, status, newState);
        if (newState == BluetoothProfile.STATE_CONNECTED) {
            gatt.discoverServices();        //连接成功， 开始搜索服务
            Log.i(TAG, "onConnectionStateChange  连接成功" + status);
        }
    }

    /**
     * Callback invoked when the list of remote services, characteristics and descriptors for the remote device have been updated, ie new services have been discovered.
     * @param gatt   返回的是本次连接的gatt对象
     * @param status
     */
    @Override
    public void onServicesDiscovered(BluetoothGatt gatt, int status) {
        super.onServicesDiscovered(gatt, status);
        Log.i(TAG, "onServicesDiscovered status" + status);
        List<BluetoothGattService> mServiceList = gatt.getServices();
        for (BluetoothGattService service : mServiceList) {
            Log.i(TAG, "onServicesDiscovered " + service.getUuid());
        }
        BluetoothGattService service = gatt.getService(UUID.fromString(GATT_SERVICE_PRIMARY_1));
        BluetoothGattCharacteristic characterisetic = service.getCharacteristic(UUID.fromString(CHARACTERISTIC_NOTIFY_1));
        //调用以便当命令发送后返回信息可以自动返回
        BluetoothGattDescriptor descriptor = characterisetic.getDescriptor(CCC);
        descriptor.setValue(BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE);
        gatt.writeDescriptor(descriptor);
        boolean isNotify = gatt.setCharacteristicNotification(characterisetic, true);
    }

	//回调响应特征写操作的结果。
    @Override
    public void onCharacteristicWrite(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
        Log.i(TAG, gatt.getDevice().getName() + " write successfully");
    }
   //回调响应特征读操作的结果。
    @Override
    public void onCharacteristicRead(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
        Log.i(TAG, gatt.getDevice().getName() + " recieved ");
    }

    @Override
    public void onCharacteristicChanged(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic) {
        super.onCharacteristicChanged(gatt, characteristic);
        Log.i(TAG, "The response is " + "onCharacteristicChanged");
    }
};
```

![](E:\noteforme.github.io\source\_posts\Bluetooth03\Android-BLE-in-Action.026.jpeg)





##### Android 中 GATT 操作的流程 

![droid-BLE-in-Acti](Bluetooth03\Android-BLE-in-Action.028.jpeg)



 Android 中 GATT 操作的流程。右边这个图，APP 是我们的应用，右边蓝牙服务端，从左向右箭头是 APP 发起的请求，从右向左的箭头是回调。我们看到所有的操作都是异步的完成的。连接过程是，首先使用 gattConnect 发起连接，收到 onConnectionStateChange() 通知连接是否成功，若成功，则进行下一步的 discoverService()，这一步就是发现设备所有的 GATT Service，若发现成功，通过 onServiceDiscovered() 回调，这时才算真正的连接成功。然后可以通过 BluetoothGatt 的 getService() 来获得BluetoothGattService，进而获得BluetoothGattCharacteristic 等，然后对 Characteristic 进行读写。 

* office

  https://developer.android.com/guide/topics/connectivity/bluetooth?hl=zh-cn

  https://developer.android.com/guide/topics/connectivity/bluetooth-le?hl=zh-cn

  [官方demo android5.0已过时](https://github.com/googlesamples/android-BluetoothLeGatt)

  https://source.android.google.cn/devices/bluetooth/

* un

  https://juejin.im/entry/5919630444d904006c6e14ca

  https://www.jianshu.com/p/29a730795294

  https://www.jianshu.com/p/046c1f5a7163

####  设备通知手机

5 当你从文档看到遍历出来的UUID有接送通知的功能。这时你就可以设置可以接收通知。通过拿到对应通知UUID的BluetoothGattCharacteristic，调用setCharacteristicNotification().其中00002902-0000-1000-8000-00805f9b34fb是系统提供接受通知自带的UUID，通过设置BluetoothGattDescriptor相当于设置BluetoothGattCharacteristic的Descriptor属性来实现通知，这样只要蓝牙设备发送通知信号，就会回调onCharacteristicChanged(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic) 方法，这你就可以在这方法做相应的逻辑处理。 

https://juejin.im/entry/58c74fc42f301e006bce23fb

#### 经典蓝牙开发

https://www.jianshu.com/p/453a5cda5646

* 设置 00002902-0000-1000-8000-00805f9b34fb

  ```
  /* 设置特征信息推送 */
  ··· BluetoothGattCharacteristic characteristic;
      mGatt.setCharacteristicNotification(characteristic,true);
  
  /* CCCD 的UUID */
  private UUID ID_CCCD = UUID.fromString("00002902-0000-1000-8000-00805f9b34fb");  
  
  /* 获取CCCD */
  BluetoothGattDescriptor cccd = characteristic.getDescriptor(ID_CCCD);
  
  /* 设置推送通知，参考值为：
   * BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE:   通知
   * BluetoothGattDescriptor.ENABLE_INDICATION_VALUE:     指示
   * BluetoothGattDescriptor.DISABLE_NOTIFICATION_VALUE:  关闭
   */  
  cccd.setValue(参考值);
  /* 写入CCCD */
  mGatt.writeDescriptor(descriptor)
  ```

  https://www.jianshu.com/p/43b1956d9f5c

  官方三个蓝牙示例 :<https://github.com/googlesamples?utf8=%E2%9C%93&q=bluetooth&type=&language=>

  #### 问题

  扫描不到任何设备

  <https://stackoverflow.com/questions/39646253/android-stops-finding-ble-devices-onclientregistered-status-133-clientif-0>