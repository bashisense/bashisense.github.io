# 策略管理
-----------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/rule
>HTTP头：token = , 使用登陆时返回的token

终端模式时：
>请求类型: POST
>object = "user"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容

**id 大小不超过32Bytes**

#### 规则对象

```json
{
    "id":"EeqZmH943F8",
    "rules" : [
        {
            "rule":"* 12 * * * *",   // crontab 规则
            "effect": 129392,   // 生效时间
            "expire": 10292929, // 过期时间
        },

        {
            "rule":"18:00-23 * * * *",   // crontab 规则
            "effect": 129392,   // 生效时间
            "expire": 10292929, // 过期时间
        },
    ]
}
```

#### 添加开门策略

- 请求

```json
{
    "action":"rule-add",
    "reqid":298299,
    "id":"EeqZmH943F8",
    "rules" : [
        {
            "rule":"* 12 * * * *",   // crontab 规则
            "effect": 129392,   // 生效时间
            "expire": 10292929, // 过期时间
        },

        {
            "rule":"18:00-23 * * * *",   // crontab 规则
            "effect": 129392,   // 生效时间
            "expire": 10292929, // 过期时间
        },
    ]
}
```

- 响应

```json
{
    "retcode":0,
    "reqid":298299,
}
```

#### 删除开门策略

- 请求

```json
{
    "action":"rule-del",
    "reqid":298299,
    "id":"EeqZmH943F8",
}
```


- 响应

```json
{
    "retcode":0,
}
```

#### 更新开门策略

- 请求

```json
{
    "action":"rule-update",
    "reqid":298299,

    "id":"EeqZmH943F8",
    "rules" : [
        {
            "rule":"* 12 * * * *",   // crontab 规则
            "effect": 129392,   // 生效时间
            "expire": 10292929, // 过期时间
        },

        {
            "rule":"18:00-23 * * * *",   // crontab 规则
            "effect": 129392,   // 生效时间
            "expire": 10292929, // 过期时间
        },
    ]
}
```

- 响应

```json
{
    "reqid":298299,
    "retcode":0,
    "rule":101
}
```

#### 列举开门策略

- 请求

```json
{
    "action":"rule-list",
    "reqid":298299,
    "offset": 0,
    "limit":10
}
```

> offset : 起始位置
> limit : 本次列举的个数

- 响应

```json
{
    "reqid":298299,
    "retcode":0,
    "params":[
        {
            "id":"EeqZmH943F8",
            "rules" : [
            {
                "rule":"* 12 * * * *",   // crontab 规则
                "effect": 129392,   // 生效时间
                "expire": 10292929 // 过期时间
            }
            ]
        },

        {    
            "id":"EeqZmH9243F1",
            "rules" : [
            {
                "rule":"* 12 * * * *",   // crontab 规则
                "effect": 129392,   // 生效时间
                "expire": 10292929, // 过期时间
            }
            ]
        }
    ]
}
```
