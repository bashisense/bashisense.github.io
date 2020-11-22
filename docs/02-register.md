# 激活登陆
设备激活是

## 1. 私有模式

设备配置成本地模式后，会启动TCP连接到私有云平台，然后进行相关交互，主要过程如下：

1. 设备启动TCP连接到智云平台，发送认证请求：

- 响应：

```json
{
    "walos-version" : "v2.1.0",             // WalOS 操作系统版本号
    "algm-version"  : "v1.1.0",             // 算法版本号
    "hardware-version" : "v1.2.0",          // 硬件载体版本号
    "device-id" : "device-id",
}
```

2. 平台端向设备发送请求，设备向云端发送响应。

见03/04/05/06/07/08/09章节；

3. 设备向平台端主动发送心跳包：

- 请求：

```json
{
    "action":"heartbeat"
}
```

- 响应：

```json
{
    "action":"heartbeat"
}
```

4. 设备向平台端发送事件消息

- 请求: 识别事件

```json
{
    "type":"logr",
    "mode": "fdr",  // fdr 人脸识别， ic 刷卡，fdric 人脸识别+刷卡
    "timestamp":1618222014,
    "device-id":"device-id",
    "userid": "1a2aUHu2EeqZmH943F8eUg==",
    // 如果有的话
    "face_x":10,
    "face_y":20,
    "face_w":200,
    "face_h":200,
    "sharp":0.95,
    "score":0.68,
    "therm":36.5
}
```

> type : 事件类似，logr识别成功；visitor访客成功； opendoor远程开门成功
> 其中除logr外，都没有拍照信息

## 2. 本地模式
设备配置成本地模式或混合模式时，会启动Web Server，提供基于Http的API接口，此时对于接放的访问需要进行认证。本地模式时的激活、登陆：
>请求URL: http://dev-ip-addr:port:/api/auth
>请求类型: POST
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错


0. 扫描设备

向同网段的IP地址的6543端口发送UDP报文：

```json
{
    "action":"scan"
}
```

如果该地址是WalOS在网设备，会响应：

```json
{
    "device-id":"device-id",
    "walos-version" : "v2.1.0",             // WalOS 操作系统版本号
    "algm-version"  : "v1.1.0",             // 算法版本号
    "hardware-version" : "v1.2.0",          // 硬件载体版本号
}
```

>此时便可以获取到目标设备的ID与协议版本信息；
>也可以通过扫描设备显示的二维码获取设备的ID信息；


1. 接入认证

- 请求：

```json
{
    "action":"auth",
    "user":"user-name",
    "passwd":"user-passwd"
}
```

- 响应：

```json
{
    "token":"823lsefhw03slej",
    "timestamp":1560392020,
    "retcode":0
}
```

>token用于后续api请求时的http header

2. 修改管理员密码

- 请求：

```json
{
    "action":"passwd",
    "user":"user-name",
    "passwd":"user-passwd"
}
```

- 响应：

```json
{
    "retcode":0
}
```

3. 添加管理员帐户

- 请求：

```json
{
    "action":"add",
    "user":"user-name",
    "passwd":"user-passwd"
}
```

- 响应：

```json
{
    "retcode":0
}
```

4. 删除管理员帐户

- 请求：

```json
{
    "action":"delete",
    "user":"user-name",
}
```

- 响应：

```json
{
    "retcode":0
}
```
