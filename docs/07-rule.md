# 策略管理

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/rule
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

智云模式时：
>消息体、返回体相关，无http相关内容

**开门策略最多不超过32个**
**id 大小不超过32Bytes**

1. 添加开门策略

- 请求

```json
{
    "action":"add",

    "id":"EeqZmH943F8",
    "mode":"week",
    "days":"0111110",
    "start":152029302,
    "end":15929203
}
```

> id : 策略ID号，自定义策略ID从100开始，100以内为保留ID号
> mode : 策略模式， week 按周循环，month 按月循环
> days : 是否使能，0表示当天不使能， 1表示当天使能
> start : 规则起始时间
> end : 规则结束时间

- 响应

```json
{
    "retcode":0,
    "rule":101
}
```

>rule : 本地序号，递增


2. 删除开门策略

- 请求

```json
{
    "action":"del",
    "id":"EeqZmH943F8",
}
```


- 响应

```json
{
    "retcode":0,
}
```

3. 更新开门策略

- 请求

```json
{
    "action":"update",

    "id":"EeqZmH943F8",
    "mode":"week",
    "days":"0111110",
    "start":152029302,
    "end":15929203
}
```

> id : 策略ID号，自定义策略ID从100开始，100以内为保留ID号
> mode : 策略模式， week 按周循环，month 按月循环
> days : 是否使能，0表示当天不使能， 1表示当天使能
> start : 规则起始时间
> end : 规则结束时间

- 响应

```json
{
    "retcode":0,
    "rule":101
}
```

4. 列举开门策略

- 请求

```json
{
    "action":"list",
    "offset": 0,
    "limit":10
}
```

> offset : 起始位置
> limit : 本次列举的个数

- 响应

```json
{
    "retcode":0,
    "params":[
        {
            "rule":102,
            "id":"EeqZmH9470",
            "mode":"month",
            "days":"01101010101010110101010",
            "start":152029302,
            "end":15929203
        },

        {
            "id":"EeqZmH63F8",
            "rule":103,
            "mode":"week",
            "days":"0111110",
            "start":158929302,
            "end":16029203
        },

        {
            "id":"EeqZmH9465",
            "rule":104,
            "mode":"week",
            "days":"0111110",
            "start":158929302,
            "end":16029203
        }
    ]
}
```
