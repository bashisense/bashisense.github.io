# 广告推送

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/advt
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

智云模式时：
>消息体、返回体相关，无http相关内容

**广告图片最多不超过128个**
**id 大小不超过32Bytes**

1. 添加广告图片

**上传广告文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**、

- 请求

```json
{
    "action":"add",
    "id":"9ssosxlsiw",
    "expire":1593282304,
    "duration":30,
    "filesize":329382,
    "offset":0,
    "size":3200,
    "data":"base64 encoded data"
}
```

>expire : 过期时间，EPOCH 秒值
>duration : 此广告播放时间时延，默认设备配置页中的广告时延值

- 响应

```json
{
    "retcode":0
}
```

2. 删除广告图片

- 请求

```json
{
    "action":"del",
    "id":"9ssosxlsiw"
}
```

- 响应

```json
{
    "retcode":0
}
```

3. 列举广告图片

- 请求

```json
{
    "action":"list",
}
```

- 响应

```json
{
    "params":[
        "9ssosxlsiw",
        "isjdwew938"
    ],
    "retcode":0
}
```
