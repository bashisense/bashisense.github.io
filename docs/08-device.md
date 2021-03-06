# 设备管理
------------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/device
>HTTP头：token = , 使用登陆时返回的token

终端模式时：
>请求类型: POST
>object = "user"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容


#### 查询设备配置

- 请求

```json
{
    "action":"device-info",
    "reqid":1839292
}
```

-响应

```json
{
    "retcode":0,
    "reqid":1839292,

    // 返回设备的配置列表
    // ....
}
```

#### 设置时间

- 请求

```json
{
    "action":"device-time",
    "reqid":1839292,
    "timezone":-480,
    "timestamp":161589233
}
```

> timezone : 时区；与格林尼治时间差的分钟数，负数为东区，正数为西区，如不填写，则使用出厂默认设备东8区
> timestamp ： 1970.1.1 开始的毫秒数；

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```

#### 设备重启

- 请求

```json
{
    "action":"device-reboot",
    "reqid":1839292,
}
```

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```


#### 设备重置

- 请求

```json
{
    "reqid":1839292,
    "action":"device-reset"
}
```

>**设备内的所有数据被清空，恢复到出厂状态，危险操作**

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```


- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```

#### 远程开门

- 请求

```json
{
    "reqid":1839292,
    "action":"device-opendoor",
    "userid":"1avsoHu2EeqZmH943F8eUg=="
}
```

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```

#### 上传升级文件

**上传文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**

- 请求

```json
{
    "reqid":1839292,
    "action":"device-upload",
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
    "reqid":1839292,
    "retcode":0
}
```

#### 升级设备

- 请求

```json
{
    "reqid":1839292,
    "action":"device-upgrade",
    "filename":"walos-v2.3.4.bin"
}
```

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```

#### 传感器快照

- 请求

```json
{
    "reqid":1839292,
    "action":"device-snapshot"
}
```

> 获取设备摄像头的快照图片

- 响应

```json
{
    "reqid":1839292,
    "retcode": 0,
    "timestamp": "1587985190"
}
```

>timestamp 为获取快照的时间戳，下载快照时需要此数据

#### 下载快照文件

- 请求

```json
{
    "reqid":1839292,
    "action":"device-download",
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
    "reqid":1839292,
    "action":"device-download",
    "type":"snapshot",
    "filesize":32678,
    "filename":"SNAP0-1584518255389.nv12",
    "offset":0,
    "size": 32000,
    "data":"base64-encoded file data"
}
```


#### 设置模式

设备运行在不同模式时，表示不同的功能，主要有两种模式：

1. 门禁模式：正常工作时进行人脸识别，识别后开门。识别期间可以通过按键进入访客二维码读码状态，读取访客码并开门。
2. 广告模式：支持广告播放方案，开启后主界面将播放广告，右下角显示人脸视频；

- 请求

```json 
{
    "reqid":1839292,
    "action":"device-mode",
    "type":"lock",
}
```

> type : lock 门锁模式， acs 门禁模式， advt 广告模式，therm 测温模式,；

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```

#### 配置网络

根据配置管理章节的说明配置好网络参数（WIFI/ETHER/LTE)后，通过以下命令使能网络配置

- 请求

```json 
{
    "reqid":1839292,
    "action":"device-apply-network",
}
```

> 此命令将会用配置内容，生成网络配置文件，将网络重新连接

- 响应

```json
{
    "reqid":1839292,
    "retcode":0
}
```


