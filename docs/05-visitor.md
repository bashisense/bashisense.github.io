# 访客管理
----------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/visitor
>HTTP头：token = , 使用登陆时返回的token

终端模式时：
>请求类型: POST
>object = "user"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容

**vid 大小不超过32Bytes， code为6位数字**

#### 访客对象

```json
{
    "vid": "98s0k9sksk",        // visitor id，全局唯一
    "code": "330669",           // 访客码，手输入密码（6位）或二维码（16位）
    "effect": 1586429594,       // 生效时间
    "expire": 1586515994        // 过期时间
}
```

#### 添加访客

- 请求

```json
{
    "object":"visitor",
    "action": "add",
    "reqid":129393,
    "params": [
        {
            "vid": "98s0k9sksk",
            "code": "330669",
            "effect": 1586429594,
            "expire": 1586515994
        }
    ]
}
```

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
    "reqid":129393,
    "retcode": 0
}
```

#### 删除访客

- 请求

```json
{
    "object":"visitor",
    "action": "del",
    "reqid":129393,
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
    "reqid":129393,
    "retcode": 0
}
```
