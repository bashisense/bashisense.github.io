# 配置管理

本地模式时：
>请求URL: http://dev-ip-addr:port:/api/config
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token
>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

智云模式时：
>消息体、返回体相关，无HTTP相关内容

1. 查询设备的配置项：

- 请求

```json
{
    "action": "list"
}
```

- 响应

```json
{
    "params": [
        {
            "key": "manufacturer",
            "modified": "2020-04-26 13:57:07",
            "options": "ro",
            "value": "BaShi Technology Ltd. co,"
        },

        ...

        {
            "key": "device-id",
            "modified": "2020-04-26 13:57:07",
            "options": "ro",
            "value": "KssDeLd5m6wYRiNXKFCHqe"
        }
    ],
    "retcode": 0
}
```

> options: ro表示只读， wr表示可配置可写，cp表壳可配置可写并且控制平台要用

2. 获取设备的配置：

- 请求

```json
{
    "action": "get",
    "params": [
        "relay-mode",
        "therm-mode",
        "algm-threshold",
        "rs485-start-data",
        "rs485-close-data"
    ]
}
```

>params 数组内的数据项为请求的配置项，支持同时获取多个配置项

- 响应

```json
{
    "params": [
        {
            "relay-mode": "enable"
        },
        {
            "therm-mode": "forehead"
        },
        {
            "algm-threshold": "0.8"
        },
        {
            "rs485-start-data": "open-door-rs485-data"
        },
        {
            "rs485-close-data": "close-door-rs485-data"
        }
    ],
    "retcode": 0
}
```

3. 配置设备

- 请求

```json
{
    "action": "set",
    "params": [
        {
            "relay-mode": "enable"
        },
        {
            "therm-mode": "wrist"
        },
        {
            "algm-threshold": "0.8"
        }
    ]
}
```

- 响应

```json
{
    "params": [
        {
            "relay-mode": "enable"
        },
        {
            "therm-mode": "wrist"
        },
        {
            "algm-threshold": "0.8"
        }
    ],
    "retcode": 0
}
```

4. 配置说明

以下为可配置数据

```json
{
    // 设备管理模式，目前支持: none, 仅提供本地http服务， cloud:接入八识智云，local：本地，TCP管理模式
    {
        "key":"manager-mode",
        "value":"none",
        "options" : "ro",
    },

    // 设备运行模式：none 无模式， lock 门锁模式， acs 门禁模式， advt 广告模式
    // 部分型号没有门锁模式、门禁模式
    {
        "key":"device-mode",
        "value":"advt",
        "options" : "cp",
    },

    // 设备使用者组织名，可自定义, 32 bytes
    {
        "key":"org-id",
        "value":"Bashi Technology",
        "options" : "wr",
    },

    // 设备门，可自定义, 32 bytes
    {
        "key":"door-id",
        "value":"main door",
        "options" : "wr",
    },


    // 智云服务器地址与端口号
    {
        "key":"cloud-server-address",
        "value":"127.0.0.1",
        "options" : "wr",
    },

    {
        "key":"cloud-server-port",
        "value":"8000",
        "options" : "wr",
    },

    // 智云平台断开后重连接时延，秒数
    {
        "key":"cloud-reconnect-duration" ,
        "value": "16",
        "options" : "wr",
    },

    // 智云平台心跳时延，秒数
    {
        "key":"hearbeat-duration" ,
        "value": "5",
        "options" : "wr",
    },

    // 本地服务端口号
    {
        "key":"local-server-port",
        "value":"8000",
        "options" : "wr",
    },

    // 本地服务模式，管理服务器地址/端口/访问路径，用于事件通知
    {
        "key":"manager-server-address",
        "value":"127.0.0.1",
        "options" : "wr",
    },

    {
        "key":"manager-server-port",
        "value":"3000",
        "options" : "wr",
    },

    {
        "key":"manager-server-path",
        "value":"report",
        "options" : "wr",
    },

    // 设备支持的最大纪录数
    {
        "key":"user-logr-max",
        "value": "40000",
        "options" : "wr",
    },

    // 提示信息显示时延
    {
        "key":"tips-duration",
        "value": "2",
        "options" : "cp",
    },

    // 广告显示时延
    {
        "key":"advertising-duration",
        "value": "10",
        "options" : "cp",
    },

    // 开门时延
    {
        "key":"door-duration",
        "value": "3",
        "options" : "cp",
    },

    // 系统信息显示时延
    {
        "key":"info-duration",
        "value":"10",
        "options" : "cp",
    },

    // 人脸识别模式：none：无算法， detect:人脸检测，reco：人脸识别，mask：口罩提醒，qrcode: 二维码
    // 当前仅支持人脸识别、人脸检测
    {
        "key":"algm-mode",
        "value":"reco",
        "options" : "ro"
    },

    // 是否支持访客：none：不支持, qrcode： 访客二维码； code：键盘访客码， mixed： 两都同时支持
    {
        "key":"visitor-mode",
        "value":"none",
        "options" : "cp",
    },

    // 是否使用RS485端口，none:不支持，door:支持开门，voice:支持语音播放，therm:支持测温输入, user: 用户透传指令
    {
        "key":"rs485-mode",
        "value":"none",
        "options" : "cp",
    },

    // 是否支持语音播放：default: 支持，默认方式； disable：不支持；user: 定制用户定制方式（不使用）
    {
        "key":"voice-mode",
        "value":"default",
        "options" : "cp",
    },

    // 无线模式： none - 不启用, 4g - 启用4G模块
    {
        "key":"cell-mode",
        "value":"none",
        "options" : "cp",
    },

    // wifi模式： none - 不启用, sta - 客户端模式， ap - 热点模式， mixed - 双模式
    {
        "key":"wifi-mode",
        "value":"mixed",
        "options" : "cp",
    },

    // 是否支持继电器开门：enable:支持, disable：不支持
    {
        "key":"relay-mode",
        "value":"enable",
        "options" : "cp",
    },

    // 是否支持测温模块: none 不支持， wrist:腕温， forehead：额温
    {
        "key":"therm-mode",
        "value":"none",
        "options" : "cp",
    },

    // 测温模块修正值
    {
        "key":"therm-fix",
        "value":"0",
        "options":"cp",
    },

    // 测温模块阈值，合格最低值
    {
        "key":"therm-normal",
        "value":"34.5",
        "options":"cp",
    },
    // 测温模块阈值，告警最低值
    {
        "key":"therm-alarm",
        "value":"37.5",
        "options":"cp",
    },

    // 是否支持事件通知：enable：支持， disable：不支持
    {
        "key":"event-mode",
        "value":"enable",
        "options" : "wr",
    },

    // 识别算法比对阈值
    {
        "key":"algm-threshold",
        "value":"0.5",
        "options" : "cp",
    },

    // 识别距离,默认1米左右，部分产品配置不支持可配置
    {
        "key":"algm-distance",
        "value":"1",
        "options" : "cp",
    },

    // 产品语言： cn - 中文， en - 英文， 支持的语言种类视硬件版本情况
    {
        "key":"lang-mode",
         "value":"cn",
         "options" : "wr"
    },

   // none - 不显示姓名， name - 仅显示姓名， desc - 显示姓名、部门。 默认desc
    {
        "key":"show-mode",
         "value":"desc",
         "options" : "cp"
    },

    // single - 任何一种认证可开门， card - 先刷卡后识别人脸再开门， face - 先识别人脸再刷卡开门， therm - 先识别人脸再测温开门
    {
        "key":"auth-mode",
         "value":"single",
         "options" : "cp"
    },

    // 仅定制版本支持
    // 门禁常开常闭策略，比如指定时段门一直打开不关闭
    {
        "key":"open-mode",
        "value":"siw879273ks",
        "options" : "wr"
    },

    // 仅定制版本支持
    // 门禁常闭策略，比如指定时段门一直关闭不打开
    {
        "key":"close-mode",
        "value":"879273ksss",
        "options" : "wr"
    },

    // 门卡类型, none - 无门禁卡支持， id - 仅读取卡原生ID匹配； en, - 加密卡（encryption）；as - 非对称加密卡（async，仅定制型号支持）
    {
        "key":"card-mode",
        "value":"id",
        "options":"cp"
    },

    // 门卡读取密钥, 6 bytes值， base64编码
    {
        "key":"card-keya",
        "value":"8279sj9",
        "options":"cp"
    },

    // 门卡写入密钥, 6 bytes值， base64编码
    {
        "key":"card-keyb",
        "value":"8skskjj==",
        "options":"cp"
    },

   // 音量配置
    {
        "key":"volume",
        "value":"100",
        "options":"cp"
    },

    // LCD屏亮度
    {
        "key":"lcd-light",
        "value":"100",
        "options":"cp"
    },

    // 红外补光灯亮度
    {
        "key":"ir-light",
        "value":"100",
        "options":"cp"
    },

    // 白光补光灯亮度
    {
        "key":"rgb-light",
        "value":"100",
        "options":"cp"
    },


    // RS485动作发送数据，分为开、关两部分
    {
        "key":"rs485-start-data",
        "value":"open-door-rs485-data",
        "options" : "cp",
    },

    {
        "key":"rs485-close-data",
        "value":"close-door-rs485-data",
        "options" : "cp",
    }
}

```
