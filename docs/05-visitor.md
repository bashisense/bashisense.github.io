# 访客管理

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/visitor
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

智云模式时：
>消息体、返回体相关，无HTTP相关内容

**vid 大小不超过32Bytes， visicode为6位数字**

1. 添加访客：

- 请求

```json
{
    "action": "add",
    "params": [
        {
            "vid": "98s0k9sksk",
            "visicode": "330669",
            "started": 1586429594,
            "expired": 1586515994
        },
        {
            "vid": "90s0k9sbca",
            "visicode": "856573",
            "started": 1586429594,
            "expired": 1586515994
        }
    ]
}
```

>vid : 唯一标志访客码；必填
>visicode : 访客输入的访客码；必填
>started : 访客允许进入开始时间； 选填，默认当前时间
>expired ：访客允许进入结束时间；选填，默认当前时间+1天

- 响应

```json
{
    "params": [
        {
            "vid": "90s0k9sbca",
            "ops":0
        },
        {
            "vid": "98s0k9sksk",
            "ops":0
        }
    ],
    "retcode": 0
}
```

2. 删除访客：

- 请求

```json
{
    "action": "del",
    "params": [
        {
            "vid": "98s0k9sksk",
        },
        {
            "vid": "90s0k9sbca",
        }
    ]
}
```

>vid : 唯一标志访客码；必填

- 响应

```json
{
    "params": [
        {
            "vid": "98s0k9sksk",
            "ops":0
        },
        {
            "vid": "90s0k9sbca",
            "ops":0
        }
    ],
    "retcode": 0
}
```
