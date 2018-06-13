---
title: Bluetooth
comments: true
date: 2018-06-11 09:41:39
tags:
categories: ANDROID
---



####  经典蓝牙 低功耗蓝牙的区别

http://www.loverobots.cn/the-analysis-is-simple-compared-with-the-classic-bluetooth-and-bluetooth-low-energy-in-android.html

[蓝牙适配建议](https://juejin.im/post/59964187f265da249600af68)



###  蓝牙基本操作

#### 蓝牙权限

```
 <uses-permission android:name="android.permission.BLUETOOTH" />
```

如果您希望您的应用启动设备发现或操作蓝牙设置，则还必须声明 `BLUETOOTH_ADMIN` 权限。 大多数应用需要此权限仅仅为了能够发现本地蓝牙设备。 除非该应用是将要应用户请求修改蓝牙设置的“超级管理员”，否则不应使用此权限所授予的其他能力。 **注**：如果要使用 `BLUETOOTH_ADMIN` 权限，则还必须拥有 `BLUETOOTH` 权限。 

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





https://juejin.im/entry/5919630444d904006c6e14ca

https://www.jianshu.com/p/29a730795294

https://developer.android.com/guide/topics/connectivity/bluetooth?hl=zh-cn

https://developer.android.com/guide/topics/connectivity/bluetooth-le?hl=zh-cn

[官方demo android5.0已过时](https://github.com/googlesamples/android-BluetoothLeGatt)