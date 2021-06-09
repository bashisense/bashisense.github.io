# 门卡管理
---------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/v3.x/acm
>HTTP头：token = , 使用登陆时返回的token

**id 大小不超过32Bytes, userid不超过32字节， rule不超过32字节**

#### 卡对象
```json
    {
        "id":"EeqZmH943F8",
        "type": "ic",       // 卡类型，默认IC卡，可以为CPU，crypto等
        "userid": "1avsoHu2EeqZmH943F8eUg==",   // 卡对应的用户ID
        "data": "..."   // 卡数据
    }
```

>id : 唯一标志卡的ID，仅内部使用，必填
>userid : 唯一标志用户的ID，不超过32字节的文本字符串；选填，离线使用时人脸+卡匹配
>type": 卡类型，默认IC卡，可以为CPU，crypto等
>data : IC卡匹配数据，必填

#### 添加门禁卡

- 请求

```json
{
    "action":"card-insert",
    
    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "cards":[
            {
            "id":"EeqZmH943F8",
            ... // 卡其它信息，详见本章卡对象说明
            },
            {
            "id":"AeqZmH943F9",
            ... // 卡其它信息，详见本章卡对象说明
            },
        ]
    }
}
```


- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "cards":[
            {
            "id":"EeqZmH943F8",
            "retcode": 0
            },
            {
            "id":"AeqZmH943F9",
            "retcode": 0
            },
        ]
    }
}
```

#### 删除门禁卡

- 请求

```json
{
    "action":"card-remove",
    
    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "cards":[
            {
            "id":"EeqZmH943F8",
            },
            {
            "id":"AeqZmH943F9",
            },
        ]
    }
}
```

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "cards":[
            {
            "id":"EeqZmH943F8",
            "retcode": 0
            },
            {
            "id":"AeqZmH943F9",
            "retcode": 0
            },
        ]
    }
}
```

#### 查询门禁卡

- 请求

```json
{
    "action":"card-lookup",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "cards":[               // 根据卡ID查询卡信息，可不填写
            {
            "id":"EeqZmH943F8",
            },
            {
            "id":"AeqZmH943F9",
            },
        ]
    }
}
```

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
    
    "body":{
        "cards":[ 
            {
            "id":"EeqZmH943F8",
            ... // 卡其它信息，详见本章卡对象说明
            },
            {
            "id":"AeqZmH943F9",
            ... // 卡其它信息，详见本章卡对象说明
            },
        ]
    }
}
```


#### 列举门禁卡

- 请求

```json
{
    "action":"card-fetch",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "offset": 18,
        "limit":2
    }
}
```

>offset : 数据库起始点，默认0
>limit : 本次列举卡数量，默认10，不超过10
>

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
    
    "body":{
        "cards":[
            {
            "id":"EeqZmH943F8",
            ... // 卡其它信息，详见本章卡对象说明
            },
            {
            "id":"AeqZmH943F9",
            ... // 卡其它信息，详见本章卡对象说明
            },
        ]
    }
}
```


#### 主动读卡

在用户使用小程序/app为用户添加卡的时候，需要读取最后一次刷卡信息，平台或app实现卡与用户的绑定，并下发到设备。此命令返回设备内最后一次刷卡器读卡数据。

- 请求

```json 
{
    "action":"card-read",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
    }
}
```

> 返回最后一次读到卡信息与时间戳

- 响应

```json
{
    "retcode":0,
    
    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "timestamp":16092820, // 读卡时间
        "type":"ic",    // 目前支持的卡类型, ic为ID卡，en为加密卡
        "data":...      // base64编码的卡信息，目前为卡ID号
    }
}
```