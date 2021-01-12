# 注册登陆
--------

#### 获取设备地址

向同网段的6543端口发送广播报文：

```json
{
    "action":"scan"
}       
```

如果该地址是WalOS在网设备，会响应：

```json
{
    "device-id":"device-id",                // 设备ID
    "hardware-version" : "v1.2.0",          // 硬件载体版本号
    "walos-version" : "v2.1.0",             // WalOS 操作系统版本号
    "protocol-version": "v2.2.0",           // 协议版本号
    "algm-vendor" : "bashi",                // 算法供应商
    "algm-version"  : "v1.1.0",             // 算法版本号
}
```

> 此时便可以获取到目标设备的ID、协议版本信息以及设备的IP地址信息；


#### 独立模式登陆
设备配置成独立模式或另外几种模式时，设备均会启动WebAPI server，提供基于Http的API接口，此时对于接放的访问需要进行认证。

>请求URL: http://dev-ip-addr:port/api/auth
>请求类型: POST
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

1. 接入认证

- 请求：

```json
{
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

## 终端模式登陆

终端模式设备分为以下几个状态：
1. 设备未连接状态：设备未连接到管理平台，可能是网络配置或服务器地址配置问题导致设备未能接入管理平台，此时设备如果有显示屏，则显示网络配置信息，提示用户配置网络以使设备可以连接到管理平台；

2. 设备已连接状态：设备已接入到管理平台，网络处于连通状态，设备与管理平台进行以下操作：
- 1. 如果设备没有签名密钥，则请求平台获取签名密钥并存储到本地；如果已有此签名密钥，则转下一步；
- 2. 如果设备没有激活码，则注册设备到平台获取激活码，存储到本地，并显示激活码二维码，供App或小程序激活；如果已有激活码则转下一步；
- 3. 设备轮询激活状态（每5秒一次），当获取设备激活时，则进入下一步，订阅平台消息；
- 4. 设备向平台发送订阅消息，订阅设备通知，设备进入正常工作状态；

初次开机的设备会进行1/2/3/4步，已激活的设备直接进行第4步操作；


1. 获取设备签名密钥

- 请求：

```json
{
    "action":"get_sign_code",
    "reqid":192839,             // 透传值，设备内唯一

    "device-id":"device-id",    // 设备ID
    "timestamp":1560392020,     // 当时时间戳
    "once" : 19283892,          // 随机数
    "signature":"xxxxx"         // 设备签名
}
```

> 第三方平台，device-id 唯一标识一台设备
> 设备签名，sha1(sort(device-id、public-key、timestamp、nonce, “signature”))。sort的含义是将参数值按照字母字典排序，然后从小到大拼接成一个字符串。public-key是云平台的公钥，所有设备与第三方平台都有的。本接口计算签名时，需要把字符串常量 “signature”参与到计算之中，区别其他场景的签名。
> public-key 采用base64编码后的串

- 响应：

```json
{
    "reqid":192839,             // 透传值，设备内唯一
    "sign-code":"xxxxxx",       // 签名码，字符串
    "retcode":0                 // 获取成功
}
```

2. 获取设备激活码


- 请求：

```json
{
    "action":"register",
    "reqid":192839,             // 透传值，设备内唯一

    "device-id":"device-id",    // 设备ID
    "timestamp":1560392020,     // 当时时间戳
    "once" : 19283892,          // 随机数
    "signature":"xxxxx"         // 设备签名
}
```

> 第三方平台，device-id 唯一标识一台设备
> 设备签名，sha1(sort(device-id、sign-code、timestamp、nonce, "register"))。sort的含义是将参数值按照字母字典排序，然后从小到大拼接成一个字符串。public-key是云平台的公钥，所有设备与第三方平台都有的。本接口计算签名时，需要把字符串常量 “register”参与到计算之中，区别其他场景的签名。

- 响应：

```json
{
    "reqid":192839,             // 透传值，设备内唯一
    "active-code":"xxxxxx",     // 激活码，字符串
    "retcode":0                 // 获取成功
}
```


3. 获取激活状态


- 请求：

```json
{
    "action":"active",
    "reqid":192839,             // 透传值，设备内唯一

    "active-code":"xxxxxx",     // 激活码，字符串
}
```

- 响应：

```json
{
    "reqid":192839,             // 透传值，设备内唯一
    "secret":"xxxxxx",          // 凭证密钥，用于登陆系统
    "retcode":0                 // 获取成功
}
```

4. 订阅设备信息

- 请求：

```json
{
    "action":"subscribe",
    "reqid":192839,         // 透传值，设备内唯一

    "secret":"xxxxxx",      // 凭证密钥，用于登陆系统

    "device-id":"device-id",    // 设备ID
    "timestamp":1560392020,     // 当时时间戳
    "once" : 19283892,          // 随机数
    "signature":"xxxxx"         // 设备签名
}
```
> 第三方平台，device-id 唯一标识一台设备
> 设备签名，sha1(sort(device-id、sign-code、timestamp、nonce, "subscribe"))。sort的含义是将参数值按照字母字典排序，然后从小到大拼接成一个字符串。public-key是云平台的公钥，所有设备与第三方平台都有的。本接口计算签名时，需要把字符串常量 “subscribe”参与到计算之中，区别其他场景的签名。

- 响应：

```json
{
    "reqid":192839,             // 透传值，设备内唯一
    "retcode":0                 // 获取成功
}
```

至此，设备进入被动模式，接收来自平台的指令，完成任务并返回响应
