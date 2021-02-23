# 门卡管理
---------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/card
>HTTP头：token = , 使用登陆时返回的token

**id 大小不超过32Bytes, userid不超过32字节， rule不超过32字节**

#### 卡对象
```json
    {
        "id":"EeqZmH943F8",
        "type": "ic",       // 卡类型，默认IC卡，可以为CPU，crypto等
        "userid": "1avsoHu2EeqZmH943F8eUg==",   // 卡对应的用户ID
        "effect":192923,
        "expire":150938992,
        "data": "..."   // 卡数据
    }
```

>id : 唯一标志卡的ID，仅内部使用，必填
>userid : 唯一标志用户的ID，不超过32字节的文本字符串；选填，离线使用时人脸+卡匹配
>rule : 开门策略，配置的开发策略； 选填，如不填侧使用默认配置，全时段可刷卡开门
>effect: 使能时间
>expire : 用户过期时间，格式需要保持一致；必填
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

