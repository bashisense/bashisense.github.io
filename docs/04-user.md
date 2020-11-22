# 帐户管理

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/user
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错
**上传用户数据时，请求URL为http://dev-ip-addr:port:/api/upload**

智云模式时：
>消息体、返回体相关，无HTTP相关内容

**限制条件： userid 不超过32字节， name不超过32字节， desc不超过32字节， others不超过32字节， rule不超过32字节**

1. 列举用户：

- 请求

```json
{
    "action": "list",
    "offset": 1,
    "limit": 2
}
```

>offset : 本次列表开始的位置；
>limit : 本次列表的数量;


- 响应

```json
{
    "params": [
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "研发部",
            "expired": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "李四.张三",
            "others": "17763548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1avsoHu2EeqZmH943F8eUg=="
        },
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "国际贸易部",
            "expired": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "张三.李四",
            "others": "18563548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1aw6wHu2EeqZmH943F8eUg=="
        }
    ],
    "retcode": 0
```

2. 添加用户：

- 请求

```json
{
    "action": "add",
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "name": "李四",
            "desc": "研发部",
            "others": "17763548853",
            "rule": "Iksiwim",
            "expire": "2019-12-09 14:52:04",
            "feature": "..."
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "name": "张三",
            "desc": "国际贸易部",
            "others": "18563548853",
            "rule": "Iksiwim",
            "expire": "2019-12-09 14:52:04",
            "feature": "..."
        }
    ]
}
```

>userid : 唯一标志用户的ID，不超过32字节的文本字符串；必填
>name : 用户名；必填
>desc : 用户部门或楼宇单元或其它说明信息； 选填
>others ： 用户手机号；选填
>rule : 开门策略，配置的开发策略； 选填，如不填侧使用默认配置，全时段可刷脸开门
>expire : 用户过期时间，格式需要保持一致；必填
>feature ： 由算法生成的特征值，选填

- 响应

```json
{
    "params": [
        { 
            "userid":"1avsoHu2EeqZmH943F8eUg==",
            "ops": 0
        },
        {
            "userid":"1aw6wHu2EeqZmH943F8eUg==",
            "ops":0
        }
    ],
    "retcode": 0
}
```

3. 删除用户

- 请求

```json
{
    "action": "del",
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg=="
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg=="
        }
    ]
}
```

>userid : 唯一标志用户的ID，不超过32字节的文本字符串；必填

- 响应

```json
{
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "ops":0
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "ops":0
        }
    ],
    "retcode": 0
}
```

4. 修改用户

- 请求

```json
{
    "action": "update",
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "name": "李四.张三",
            "desc": "销售部",
            "others": "13663548853",
            "rule": "Iksiwim",
            "expire": "2019-12-09 14:52:04",
            "feature": "..."
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "name": "张三.李四",
            "desc": "国际贸易部",
            "others": "13663548853",
            "rule": "Iksiwim",
            "expire": "2019-12-09 14:52:04",
            "feature": "..."
        }
    ]
}
```
>userid : 唯一标志用户的ID，不超过32字节的文本字符串；必填
>name : 用户名；选填
>desc : 用户部门或其它说明信息； 选填
>others ： 用户手机号；选填
>rule : 开门策略，配置的开发策略； 选填，如不填侧使用默认配置，全时段可刷脸开门
>expire : 用户过期时间，格式需要保持一致；选填
>feature ： 由算法生成的特征值，选填

- 响应

```json
{
    "params": [
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "销售部",
            "expired": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "李四.张三",
            "others": "13663548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "ops":0
        },
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "国际贸易部",
            "expired": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "张三.李四",
            "others": "13663548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "ops":0
        }
    ],
    "retcode": 0
}
```

5. 查询用户

- 请求

```json
{
    "action": "info",
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg=="
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg=="
        }
    ]
}
```

>userid : 唯一标志用户的ID，不超过32字节的文本字符串；必填

- 响应

```json

{
    "params": [
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "销售部",
            "expired": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "李四.张三",
            "others": "13663548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "ops":0
        },
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "国际贸易部",
            "expired": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "张三.李四",
            "others": "13663548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "ops":0
        }
    ],
    "retcode": 0
}
```

6. 上传用户文件

**上传用户文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**

- 请求

```json
{
    "action":"upload",
    "filesize":32678,
    "filename":"1aw6wHu2EeqZmH943F8eUg==.nv12",
    "offset":4096,
    "size": 4096,
    "data":"base64-encoded file data"
}
```

>type : 上传数据的类型，用户文件
>filename : 用户ID + 后缀
>filesize : 文件总的长度
>offset : 当前数据片段起始位置
>size : 当前数据片段长度
>data : base64编码的数据

- 响应

```json
{
    "retcode":0
}
```

7. 生成用户文件

根据上传的用户nv12/nv21数据，生成所需要的UI或识别特征码数据

- 请求

```json
{
    "action": "gen",
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "type":"feature"
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "type":"all"
        }
    ]
}
```

>type : feature 用当前的算法生成feature数据， ui 生成UI所需的图片， all 生成两者
>userid : 指定用户的ID号

- 响应

```json
{
    "retcode":0,
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg==",
            "ops":0
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
            "ops":0
        }
    ]
}
```

8. 下载帐户文件

- 请求

```json
{
    "action":"download",
    "filename":"1aw6wHu2EeqZmH943F8eUg==.nv12",
    "offset":0,
    "size": 4096,
}
```

> filename : 用户ID+文件后缀名，.nv12 上传的用户文件, .bmp 生成的UI文件

- 响应

```json
{
    "action":"pushdata",
    "filename":"1aw6wHu2EeqZmH943F8eUg==.nv12",
    "filesize":32678,
    "offset":0,
    "size": 32000,
    "data":"base64-encoded file data"
}
```
