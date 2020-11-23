# 事件管理
---------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/logr
>HTTP头：token = , 使用登陆时返回的token

终端模式时：
>请求类型: POST
>object = "user"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容

#### 事件对象

```json
{
    "seqnum": 6,                // 事件编号，设备唯一
    "timestamp": 1587985190,    // 事件发生时的时间戳
    "type":"logr",          // 事件类型，logr-识别，open - 开门状态， remote - 远程开门
                            // fire -火警告警， disa - 防拆告警， card - 刷卡， visitor 访客，

    "score": 1,         //  识别相似度
    "sharp": 0,         //  照片清晰度
    "therm": 0,         //  测温值
    "userid": "TUID-876991", // 识别出的用户ID，或陌生人 0000开头的ID
    "data":"...",       // base64编码的其它值（此值暂不填写）
}
````

#### 获取日志信息

- 请求

```json
{
    "object":"event",
    "action":"info",
    "reqid":82929,
}
```

- 响应

```json
{
    "max": 9,
    "min": 1,
    "reqid":82929,
    "retcode": 0
}
```

#### 获取日志数据

- 请求

```json
{
    "object":"event",
    "action":"fetch",
    "reqid":82929,
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
            "score": 1,
            "seqnum": 6,
            "sharp": 0,
            "therm": 0,    
            "timestamp": 1587985190,
            "userid": "TUID-876991"
        },
        {
            "score": 1,
            "seqnum": 7,
            "sharp": 0,
            "therm": 0, 
            "timestamp": 1587983490,
            "userid": "TUID-1039100"
        }
    ],
    "reqid":82929,
    "retcode": 0
}
```

#### 下载日志图片

- 请求

```json
{
    "object":"event",
    "action":"download",
    "reqid":82929,
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
    "reqid":82929,
    "type":"logr",
    "filesize":32678,
    "filename":"1587983490.jpg",
    "offset":0,
    "size": 32000,
    "data":"base64-encoded file data"
}
```

#### 日志通知

- 请求

```json
{
    "object":"event",
    "action":"event",

    "device-id":"device-id",    // 发出事件的设备ID
    "seqnum": 6,                // 当前事件的序号
    "type":"logr",              // 事件的类型
    "timestamp":1587983490,     // 事件发生的时间戳，EPOCH时间*1000 + 毫秒数
    "sharp":0.983,  // 人脸清晰度，如果有，仅logr情况下有意义
    "score":0.834,  // 人脸识别的相似度，如果有，仅logr情况下有意义
    "therm":36.5,   // 体温值，如果有，仅logr情况下有意义
    "userid":"USERID028SL0LSLS",    // 识别出的人脸ID，0000开头的为陌生人，仅logr情况下有意义
    "username":"张三",  // 识别出的人名，部分版本/设备没有此项
    "data":base64(data) // 事件附件消息，如果有的话
}
```
