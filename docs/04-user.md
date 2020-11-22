# 帐户管理
-----------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/user
>HTTP头：token = , 使用登陆时返回的token

其它模式时：
>请求类型: POST
>object = "user"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容

**限制条件： userid 不超过32字节， name不超过32字节， desc不超过32字节， others不超过32字节， rule不超过32字节**

#### 用户对象

```json
{
    "userid": "1avsoHu2EeqZmH943F8eUg",     // 用户ID，全局唯一
    "name": "李四",     // 用户姓名，必填
    "desc": "研发部",   // 用户部门/住宅门牌号等，可不填写
    "others": "17763548853",    // 附加信息，可用于梯控等场景，可不填写
    "effect": 1619019996,       // 用户生效时间，不填则当前时刻，EPOCH值
    "expire": 1619519996,       // 用户过期时间，不填则当前时刻+1年
    "rule": "Iksiwim",          // 用户开门规则，默认识别即通过，可不填写
    "state": 0,                 // 用户状态，暂内部使用，可不填写
    "feature": "data...."       // base64编码的用户特征值，由设备计算，平台不填写
}
```

对用户的操作与数据库的CRUD类似，增加了枚举设备中的所有用户，所有的信息以此为准

#### 列举用户

- 请求

```json
{
    "object":"user",
    "action": "list",
    "reqid":129393,
    "offset": 1,
    "limit": 2
}
```

>offset : 本次列表开始的位置；
>limit : 本次列表的数量;


- 响应

```json
{
    "reqid":129393,
    "retcode": 0,
    "params": [
        {
            "create_datetime": "2020-04-27 18:39:56",
            "desc": "研发部",
            "effect": 1609519996,
            "expire": 1619519996,
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
            "effect": 1609519996,
            "expire": 1619519996,
            "modify_datetime": "2020-04-27 18:39:56",
            "name": "张三.李四",
            "others": "18563548853",
            "rule": "Iksiwim",
            "state": 0,
            "userid": "1aw6wHu2EeqZmH943F8eUg=="
        }
    ],
```

#### 添加用户

- 请求

```json
{
    "object":"user",
    "action": "add",
    "reqid":129393,
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
    "reqid":129393,
    "retcode": 0
}
```

#### 删除用户

- 请求

```json
{
    "object":"user",
    "action": "del",
    "reqid":129393,
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
    "reqid":129393,
    "retcode": 0
}
```

#### 修改用户

- 请求

```json
{
    "object":"user",
    "action": "update",
    "reqid":129393,
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
    "reqid":129393,
    "retcode": 0
}
```

#### 查询用户

- 请求

```json
{
    "object":"user",
    "action": "info",
    "reqid":129393,
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
    "reqid":129393,
    "retcode": 0
}
```

#### 上传用户文件

**上传用户文件，单次数据长度必须在256kB以内，其data数据在转base64前应以256kB长度切片**

- 请求

```json
{
    "object":"user",
    "action":"upload",
    "reqid":129393,

    "filesize":32678,
    "filename":"1aw6wHu2EeqZmH943F8eUg==.nv12",
    "offset":4096,
    "size": 4096,
    "data":"base64-encoded file data"
}
```

>filename : 用户ID + 后缀, nv12时为nv12文件，png时为PNG文件，使用512x512的人脸图片，做底库使用
>filesize : 文件总的长度
>offset : 当前数据片段起始位置
>size : 当前数据片段长度
>data : base64编码的数据

- 响应

```json
{
    "reqid":129393,
    "retcode":0
}
```

#### 生成用户文件

根据上传的用户数据，生成所需要的UI与识别特征码数据

- 请求

```json
{
    "object":"user",
    "action": "gen",
    "reqid":129393,
    "params": [
        {
            "userid": "1avsoHu2EeqZmH943F8eUg==",
        },
        {
            "userid": "1aw6wHu2EeqZmH943F8eUg==",
        }
    ]
}
```

- 响应

```json
{
    "retcode":0,
    "reqid":129393,
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

#### 下载帐户文件

- 请求

```json
{
    "object":"user",
    "action":"download",
    "reqid":129393,
    "filename":"1aw6wHu2EeqZmH943F8eUg==.nv12",
    "offset":0,
    "size": 4096,
}
```

> filename : 用户ID+文件后缀名，.nv12/.png 上传的用户文件, .bmp 生成的UI文件

- 响应

```json
{
    "reqid":129393,

    "filename":"1aw6wHu2EeqZmH943F8eUg==.nv12",
    "filesize":32678,
    "offset":0,
    "size": 32000,
    "data":"base64-encoded file data"
}
```
