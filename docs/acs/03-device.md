# 设备管理
------------

#### 说明

>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/v3.x/device
>HTTP头：token = , 使用登陆时返回的token

#### 设置时间

- 请求

```json
{
    "action":"sync-time",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
    
    "body":{
        "timezone":-480,
        "timestamp":161589233
    }
}
```

> timezone : 时区；与格林尼治时间差的分钟数，负数为东区，正数为西区，如不填写，则使用出厂默认设备东8区
> timestamp ： 1970.1.1 开始的秒数；

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

#### 设备重启

- 请求

```json
{
    "action":"device-reboot",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```


#### 设备重置

- 请求

```json
{
    "action":"device-reset",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

>**设备内的所有数据被清空，恢复到出厂状态，危险操作**

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

#### 远程开门

- 请求

```json
{
    "action":"device-opendoor",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "userid":"1avsoHu2EeqZmH943F8eUg==",
        "type":"open",   // "open" - 开门动作， "close“关门动作；不填默认为开门
        "delay":12,     // 延时多久开门，不填只默认用户识别开门时间，-1为常开或常闭
    }
}
```

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

#### 上传升级文件

**上传文件，单次数据长度必须在64kB以内，其data数据在转base64前应以64kB长度切片**

- 请求

```json
{
    "action":"device-upload",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "type":"upgrade",
        "filesize":32678,
        "filename":"walos-v2.3.4.bin",
        "offset":4096,
        "size": 4096,
        "data":"base64-encoded file data"
    }
}
```

>type : 上传数据的类型，升级文件
>filename : 升级文件名
>filesize : 文件总的长度
>offset : 当前数据片段起始位置
>size : 当前数据片段长度
>data : base64编码的数据

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

#### 升级设备

- 请求

```json
{
    "action":"device-upgrade",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "filename":"walos-v2.3.4.bin"
    }
}
```

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    }
}
```

#### 传感器快照

- 请求

```json
{
    "action":"device-snapshot",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
}
```

> 获取设备摄像头的快照图片

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body":{
        "timestamp": "1587985190"   // 快照时间戳，用于读取快照的ID
    }
}
```

>timestamp 为获取快照的时间戳，下载快照时需要此数据

#### 下载快照文件

- 请求

```json
{
    "action":"device-download",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
    
    "body":{
        "type":"snapshot",
        "chanel":0,
        "offset":0,
        "size": 32000,
        "timestamp":1584518255389
    }
}
```

> timestamp : 生成快照时返回的时间戳
> chanel : 准备获取的摄像头通道，目前仅支持chanel 0，红外通道暂不支持

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
    
    "body": {
        "type":"snapshot",
        "filesize":32678,
        "filename":"SNAP0-1584518255389.jpg",
        "offset":0,
        "size": 32000,
        "data":"base64-encoded file data"
    }
}
```

#### 配置网络

根据配置管理章节的说明配置好网络参数（WIFI/ETHER/LTE)后，通过以下命令使能网络配置

- 请求

```json 
{
    "action":"apply-network",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
}
```

> 此命令将会用配置内容，生成网络配置文件，将网络重新连接

- 响应

```json
{
    "retcode":0,

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
}
```


#### 配置设备模式

根据配置管理章节的说明配置好设备模式，通过以下命令清楚当前数据，并切换模式

- 请求

```json 
{
    "action":"apply-manager-mode",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
}
```

> 此命令将会重置设备，切换到指定的模式，当设备再次重置时，会保留此默认模式

- 响应

```json
{
    "retcode":0,
    
    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },
}
```


#### 人脸识别起停

用户使用小程序/app控制设备开始/停止人脸识别，用于特殊场景的人脸识别控制

- 请求

```json 
{
    "action":"algm-control",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
        "type":"fdr",       // fdr - 人脸识别
        "control":"start" // start,开始； stop,暂停； state,获取状态
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
        "type":"fdr",
        "state":"running"     // 返回算法引擎状态: running, stop
    }
}
```

#### 应用算法配置

将配置项中algm-* 算法引擎配置项应用到算法引擎，如果不调用此接口，配置的算法参数将在重启后升效

- 请求

```json 
{
    "action":"apply-algm-conf",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {}
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
    }
}
```

#### 打印运行日志到远程

- 请求

```json 
{
    "action":"log-toremote",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
        "remote":"192.168.2.100"    // 远程服务器地址
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
    }
}
```

#### 打印运行日志到文件

- 请求

```json 
{
    "action":"log-tofile",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
        "enable":1    // 0 - 关闭打印到文件， 1 - 开启打印到文件
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
    }
}
```

#### 下载运行日志文件

- 请求

```json 
{
    "action":"log-download",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
        "logfile": "logger.a",   // logger.a 或 logger.b 系统会交替使用这两个文件，打印超过10MB时切换
        "offset": 0,    // 读文件的偏移
        "size":1024,    // 读文件的大小，不超过64KB每次
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
        "offset": 0,    // 读文件的偏移
        "size":1024,    // 本次读到的大小
        "data":"sasdads...", // Base64编码的文件数据
        "filename":"logger.a", // 本次读取的文件名
        "filesize":123928,  // 运行日志文件大小
    }
}
```

#### 配置4G APN

- 请求

```json 
{
    "action":"apn-config",

    "header":{
        "reqid":"129393"         // 透传值，设备内唯一
    },

    "body": {
        "apn":"public.vpdn",
        "user":"admin@xxxx.com",
        "pass":"@skd9kwdkk",
        "auth":0, // 0=none，1=pap，2=chap，3=pap，chap）
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
    }
}
```