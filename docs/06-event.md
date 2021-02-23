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

#### 事件对象

```json
{
    "seqnum": 6,                // 事件编号，设备唯一
    "timestamp": 1587985190,    // 事件发生时的时间戳, 精确到毫秒

    "type":"logr",          // 事件类型，logr-识别，open - 开门状态， remote - 远程开门
                            // fire -火警告警， disa - 防拆告警， card - 刷卡， visitor 访客，
    
    // 访客事件包括此项
    "vid": "9sdkskw9", 

    // 人脸识别事件包括下面三项
    "userid": "TUID-876991", // 识别出的用户ID，或陌生人 0000开头的ID
    "score": 1,         //  识别相似度
    "sharp": 0,         //  照片清晰度

    // 测温应用包括此项
    "therm": 0,         //  测温值

    // 刷卡事件包括此项
    "data":"...",       // base64编码的刷卡值
}
````

#### 获取日志信息

- 请求

```json
{
    "action":"event-lookup",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

- 响应

```json
{
    "retcode": 0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
    
    "body":{
        "max": 9,
        "min": 1,
    }
}
```

#### 获取日志数据

- 请求

```json
{
    "action":"event-fetch",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "seqnum":6,
        "count":2
    }
}
```

>seqnum : 起始日志序号；选填，默认最小日志序号
>count ： 获取条数；选填，默认10条

- 响应

```json
{
    "retcode": 0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
        "events":[
        {
            "seqnum": 6,
            ... // 事件的其它信息，请参照本章的事件对象说明
        },
        {
            "seqnum": 7,
            ... // 事件的其它信息，请参照本章的事件对象说明
        }
        ],
    }
}
```

#### 下载日志图片

- 请求

```json
{
    "action":"event-download",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "timestamp": 1587983490,
        "type":"logr",
        "offset":0,
        "size": 32000
    }
}
```

> timestamp : 日志生成的时间戳，下载此日志对应的图片，如果有的话

- 响应

```json
{
    "retcode": 0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "type":"logr",
        "filesize":32678,
        "filename":"1587983490.jpg",
        "offset":0,
        "size": 32000,
        "data":"base64-encoded file data"
    }
}
```

#### 日志通知

- 请求

```json
{
    "action":"event-report",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "device-id":"device-id",    // 发出事件的设备ID
        "seqnum": 6,                // 当前事件的序号
        ... // 事件的其它信息，请参照本章的事件对象说明
    }
}
```
