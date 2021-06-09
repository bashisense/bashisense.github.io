# 事件管理
---------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/v3.x/event
>HTTP头：token = , 使用登陆时返回的token

#### 事件对象

```json
{
    "seqnum": 6,                // 事件编号，设备唯一
    "timestamp": 1587985190,    // 事件发生时的时间戳, 精确到毫秒

    "event":"face",          // 事件类型：face -人脸识别，door - 开门状态， remote - 远程开门, push - 按键开门， qrcode - 扫码事件
                            // fire -火警告警， disa - 防拆告警， reset - 复位，card - 刷卡， visitor 访客， therm - 测温
    "sync":0,               // 是否已向服务器同步，仅企业微信版本有效

    // 访客事件包括此项
    "vid": "9sdkskw9", 

    // 人脸识别事件包括下面三项
    "uid": "TUID-876991", // 识别出的用户ID，或陌生人 0000开头的ID, 卡事件也包括此项
    "score": 1,         //  识别相似度
    "sharp": 0,         //  照片清晰度

    // 测温应用包括此项
    "therm": 0,         //  测温值

    // 刷卡事件包括此项
    "data":"...",       // 卡ID值, 0xffae0846

    // open/remote/push/fire/disa 事件包括此项
    "state":0,           // 告警事件场景：0 - 关闭 / 1 - 触发 ; 人脸/刷卡场景，表示用户状态

    // 扫码事件包括此项
    "qrcode":"aiwksieow"    // 扫码值
}
```

#### 获取日志信息

- 请求

```json
{
    "action":"event-info",

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
        "limit":2
    }
}
```

> seqnum : 起始日志序号；选填，默认最小日志序号
> offset : 起始偏移；选填，默认为0
> limit  : 单次条数，默认10条


**seqnum与offset为二选一参数，如果全部都填写，默认为是seqnum; 如果全不填写，默认为offset = 0**


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