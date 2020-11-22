# 注册登陆
--------

## 获取设备地址

向同网段的6543端口发送广播报文：

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


## 独立模式登陆
设备配置成独立模式或另外几种模式时，设备均会启动WebAPI server，提供基于Http的API接口，此时对于接放的访问需要进行认证。

>请求URL: http://dev-ip-addr:port/api/auth
>请求类型: POST
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

1. 接入认证

- 请求：

```json
{
    "object":"auth",
    "action":"auth",
    "reqid":192839,         // 透传值，设备内唯一
    "user":"admin",
    "passwd":"admin-passwd"
}
```

- 响应：

```json
{
    "token":"823lsefhw03slej",
    "timestamp":1560392020,
    "reqid":192839,         // 透传值，设备内唯一
    "retcode":0
}
```

>token用于后续api请求时的http header的token字段，后续的访问必须设置 Token 字段

2. 修改管理员密码

- 请求：

```json
{
    "object":"auth",
    "action":"passwd",
    "reqid":192839,         // 透传值，设备内唯一
    "user":"admin",
    "passwd":"admin-passwd"
}
```

- 响应：

```json
{
    "reqid":192839,         // 透传值，设备内唯一
    "retcode":0
}
```

3. 修改其它模式的登入帐户密码

其它模式时，设备需要登陆第三方平台，帐号可以通过配置软件配置，也可以通过API软件添加

- 请求：

```json
{
    "object":"auth",
    "action":"passwd",
    "reqid":192839,         // 透传值，设备内唯一
    "user":"walos",
    "passwd":"passwd"
}
```

- 响应：

```json
{
    "reqid":192839,         // 透传值，设备内唯一
    "retcode":0
}
```

## 其它模式登陆


- 请求：

```json
{
    "object":"auth",
    "action":"auth",
    "reqid":192839,         // 透传值，设备内唯一
    "user":"walos",
    "passwd":"walos-passwd",
    "device-id":"device-id",
    "walos-version" : "v2.1.0",             // WalOS 操作系统版本号
    "algm-version"  : "v1.1.0",             // 算法版本号
    "hardware-version" : "v1.2.0",          // 硬件载体版本号
}
```

> 第三方平台，可以通过用户/密码/设备id三元组唯一标识一台设备
> 登陆请求含用版本信息，用于平台确认设备是否是允许接入的版本

- 响应：

```json
{
    "token":"823lsefhw03slej",              // 其它模式的http请求时，会在header中添加  Token字段，携带此值
    "timestamp":1560392020,
    "reqid":192839,         // 透传值，设备内唯一
    "retcode":0
}
```

