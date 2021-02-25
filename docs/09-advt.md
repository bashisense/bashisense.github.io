# 广告推送
----------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/v3.x/acm
>HTTP头：token = , 使用登陆时返回的token

**id 大小不超过32Bytes**

#### 添加广告图片

**上传广告文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**、

- 请求

```json
{
    "action":"advt-insert",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "id":"9ssosxlsiw",
        "effect":1929292,
        "expire":1593282304,
        "duration":30,
        "filesize":329382,
        "offset":0,
        "size":3200,
        "data":"base64 encoded data"
    }
}
```

>expire : 过期时间，EPOCH 秒值
>duration : 此广告播放时间时延，默认设备配置页中的广告时延值

- 响应

```json
{
    "retcode":0,
    
    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
}
```

#### 删除广告图片

- 请求

```json
{
    "action":"advt-remove",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "id":"9ssosxlsiw"
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
}
```

#### 查询广告图片

- 请求

```json
{
    "action":"advt-lookup",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "ads":[
            { "id":"9ssosxlsiw" },
            { "id":"isjdwew938" }
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
        "ads":[
            {
                "id":"9ssosxlsiw"
                ... // 其它信息，参考本章头部广告对象信息
            },
            {
                "id":"isjdwew938"
                ... // 其它信息，参考本章头部广告对象信息
            }
        ]
    }
}
```

#### 列举广告图片

- 请求

```json
{
    "action":"advt-fetch",
    
    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "offset":0,
        "limit":10
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
        "ads":[
            {
                "id":"9ssosxlsiw"
                ... // 其它信息，参考本章头部广告对象信息
            },
            {
                "id":"isjdwew938"
                ... // 其它信息，参考本章头部广告对象信息
            }
        ]
    }
}
```
