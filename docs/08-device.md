# 设备管理

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/device
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

智云模式时：
>消息体、返回体相关，无http相关内容

设备管理接口：
0. 查询设备信息：

- 请求

```json
{
    "action":"info",
}
```

-响应

```json
{
    "retcode":0,
    // 硬件信息

    // 存储信息

    // 网络信息
    "type":"eth",
    "mode":"static",
    "ipv4-address":"192.168.20.10",
    "ipv4-mask":"255.255.255.0",
    "ipv4-gateway":"192.168.20.1",
    "ipv4-dns":"114.114.114.114"
}
```

1. 设置时间：

- 请求

```json
{
    "action":"time",
    "timezone":-480,
    "timestamp":161589233
}
```

> timezone : 时区；与格林尼治时间差的分钟数，负数为东区，正数为西区，如不填写，则使用出厂默认设备东8区
> timestamp ： 1970.1.1 开始的毫秒数；

- 响应

```json
{
    "retcode":0
}
```

2. 设备重启

- 请求

```json
{
    "action":"reboot"
}
```

- 响应

```json
{
    "retcode":0
}
```


3. 设备重置

- 请求

```json
{
    "action":"reset"
}
```

>**设备内的所有数据被清空，恢复到出厂状态，危险操作**

- 响应

```json
{
    "retcode":0
}
```

4. 配置网络

- 请求

```json
{
    "action":"netconfig",
    "type":"wlan0",
    "mode":"static",
    "ipv4-address":"192.168.20.10",
    "ipv4-mask":"255.255.255.0",
    "ipv4-gw":"192.168.20.1",
    "ipv4-dns":"114.114.114.114",
    "wifi-ssid":"chinanet-18sasd29",
    "wifi-passwd": "1234567890",
    "wifi-ap-ssid":"bwa-9slsdk",
    "wifi-ap-passwd":"1234567890",
}
```

> type: 应用的配置类似：eth0 - 以太网口， wlan0 - wifi配置IP配置，wifi 热点配置
> eth0, wlan0参数为：

> 1. mode: staticip 静态网址；dynamic 动态网址; none 禁用网络；
> 2. ipv4-address: 静态配置时， IPv4地址
> 3. ipv4-mask: 静态配置时，网址掩码
> 4. ipv4-gateway: 静态配置时，网关地址
> 5. ipv4-dns: 静态配置时，dns服务器地址

> wifi 参数为：

> 1. wifi-ssid : 待连接的WIFI 热点名
> 2. wifi-passwd : 待连接的WIFI热点密码
> 3. wifi-ap-ssid : 本机做为热点时WIFI名称
> 4. wifi-ap-passwd : 本机做为热点时，WIFI密码


- 响应

```json
{
    "retcode":0
}
```

5. 远程开门

- 请求

```json
{
    "action":"opendoor",
    "userid":"1avsoHu2EeqZmH943F8eUg=="
}
```

- 响应

```json
{
    "retcode":0
}
```

6. 上传升级文件

**上传文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**

- 请求

```json
{
    "action":"upload",
    "type":"upgrade",
    "filesize":32678,
    "filename":"walos-v2.3.4.bin",
    "offset":4096,
    "size": 4096,
    "data":"base64-encoded file data"
}
```

>type : 上传数据的类型，升级文件
>filename : 升级文件名
>filesize : 文件总的长度
>offset : 当前数据片段起始位置
>size : 当前数据片段长度
>data : base64编码的数据

- 响应

```json
{
    "retcode":0
}
```

7. 升级设备

- 请求

```json
{
    "action":"upgrade",
    "filename":"walos-v2.3.4.bin"
}
```

- 响应

```json
{
    "retcode":0
}
```

8. 传感器快照

- 请求

```json
{
    "action":"snapshot"
}
```

> 获取设备摄像头的快照图片

- 响应

```json
{
    "retcode": 0,
    "timestamp": "1587985190"
}
```

>timestamp 为获取快照的时间戳，下载快照时需要此数据

9. 下载快照文件

- 请求

```json
{
    "action":"download",
    "type":"snapshot",
    "chanel":0,
    "offset":0,
    "size": 32000,
    "timestamp":1584518255389
}
```

> timestamp : 生成快照时返回的时间戳
> chanel : 准备获取的摄像头通道

- 响应

```json
{
    "action":"download",
    "type":"snapshot",
    "filesize":32678,
    "filename":"SNAP0-1584518255389.nv12",
    "offset":0,
    "size": 32000,
    "data":"base64-encoded file data"
}
```


10. 设置模式

设备运行在不同模式时，表示不同的功能，主要有四种模式：

1. 门锁模式：平时设备不点亮屏显与补光灯，当触摸按键时，点亮屏显+补光灯，识别人脸后开门或者读取开锁二维码后开门；
2. 门禁模式：正常工作时进行人脸识别，识别后开门。识别期间可以通过按键进入访客二维码读码状态，读取访客码并开门。
3. 测温模式：分为腕温测温与额温测温，两种测温模块可选，对于识别部分可以是人脸识别开门或口罩提醒开门，但不建议同时进行。
4. 广告模式：支持广告播放方案，开启后主界面将播放广告，右下角显示人脸视频；

- 请求

```json 
{
    "action":"mode",
    "type":"lock",
}
```

> type : lock 门锁模式， acs 门禁模式， advt 广告模式，therm 测温模式,；

- 响应

```json
{
    "retcode":0
}
```

