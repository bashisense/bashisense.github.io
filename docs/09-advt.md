# 广告推送
----------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/advt
>HTTP头：token = , 使用登陆时返回的token

终端模式时：
>请求类型: POST
>object = "user"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容

**id 大小不超过32Bytes**

#### 添加广告图片

**上传广告文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**、

- 请求

```json
{
    "object":"advt",
    "reqid":19292,
    "action":"add",

    "id":"9ssosxlsiw",
    "effect":1929292,
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
    "reqid":19292,
    "retcode":0
}
```

#### 删除广告图片

- 请求

```json
{
    "object":"advt",
    "reqid":19292,
    "action":"del",
    "id":"9ssosxlsiw"
}
```

- 响应

```json
{
    "reqid":19292,
    "retcode":0
}
```

#### 列举广告图片

- 请求

```json
{
    "object":"advt",
    "reqid":19292,
    "action":"list",
}
```

- 响应

```json
{
    "reqid":19292,
    "params":[
        "9ssosxlsiw",
        "isjdwew938"
    ],
    "retcode":0
}
```
