# 日志管理

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/logr
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

智云模式时：
>消息体、返回体相关，无HTTP相关内容

1. 获取日志信息：

- 请求

```json
{
    "action":"info"
}
```

- 响应

```json
{
    "max": 9,
    "min": 1,
    "retcode": 0
}
```

2. 获取日志数据：

- 请求

```json
{
    "action":"fetch",
    "seqnum":6,
    "count":2
}
```

>seqnum : 起始日志序号；选填，默认最小日志序号
>count ： 获取条数；选填，默认10条

- 响应

```json
{
    "logger": [
        {
            "face_h": 0,
            "face_w": 0,
            "face_x": 0,
            "face_y": 0,
            "score": 1,
            "seqnum": 6,
            "sharp": 0,
            "therm": 0,    
            "timestamp": 1587985190,
            "userid": "TUID-876991"
        },
        {
            "face_h": 0,
            "face_w": 0,
            "face_x": 0,
            "face_y": 0,
            "score": 1,
            "seqnum": 7,
            "sharp": 0,
            "therm": 0, 
            "timestamp": 1587983490,
            "userid": "TUID-1039100"
        }
    ],
    "retcode": 0
}
```

3. 下载日志图片

- 请求

```json
{
    "action":"download",
    "type":"logr",
    "timestamp": 1587983490,
    "offset":0,
    "size": 32000
}
```

> timestamp : 日志生成的时间戳

- 响应

```json
{
    "action":"download",
    "type":"logr",
    "filesize":32678,
    "filename":"1587983490.jpg",
    "offset":0,
    "size": 32000,
    "data":"base64-encoded file data"
}
```

4. 日志通知

- 云端模式时，在连接并认证后，发生事件，WalOS是直接通知云端，云端无需返回事件通知响应
- 本地模式时，当配置了"manager-server-address","manager-server-port","manager-server-path",后，事件会通过HTTP请求POST方式请求URL：http://manager-server-address:manager-server-port/manager-server-path，消息体为JSON格式

- 请求

```json
{
    "device-id":"device-id",    // 发出事件的设备ID
    "seqnum": 6,                // 当前事件的序号
    "type":"logr",              // 事件的类型： logr - 人脸识别， card 刷卡， opendoor远程开门成功， door-open开门状态， door-close关门状态，visitor访客进入
    "timestamp":1587983490,     // 事件发生的时间戳，EPOCH时间*1000 + 毫秒数
    "sharp":0.983,  // 人脸清晰度，如果有，仅logr情况下有意义
    "score":0.834,  // 人脸识别的相似度，如果有，仅logr情况下有意义
    "therm":36.5,   // 体温值，如果有，仅logr情况下有意义
    "userid":"USERID028SL0LSLS",    // 识别出的人脸ID，0000开头的为陌生人，仅logr情况下有意义
    "username":"张三",  // 识别出的人名，部分版本/设备没有此项
    "data":base64(data) // 事件附件消息，如果有的话
}
```
