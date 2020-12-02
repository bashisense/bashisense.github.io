# 配置管理
------

## 说明

>请求消息体:JSON
>返回消息体:JSON, retcode = 0时正确， 非0时出错

独立模式时：
>请求URL: http://dev-ip-addr:port:/api/config
>请求类型: POST
>HTTP头：token = , 使用登陆时返回的token

终端模式时：
>请求类型: POST
>object = "config"
>HTTP头：token = , 使用登陆时返回的token
>消息体、返回体相关，无HTTP相关内容

#### 查询设备的配置项：

- 请求

```json
{
    "action": "config-list",
    "reqid":192839         // 透传值，设备内唯一
}
```

- 响应

```json
{
    "params": [
    {
        "key":"org-id",
        "value":"Bashi Technology",
        "class":"interactive",
        "desc" : "组织，与GMT 0 相差的分钟数",
        "options" : "cp"
    },
        ...
    {
        "key":"door-id",
        "value":"Build1 Door2",
        "class":"interactive",
        "desc" : "当前门禁机管理的门的名称",
        "options" : "cp"
    },
    ],
    "reqid":192839,         // 透传值，设备内唯一
    "retcode": 0
}
```

>  options: wr表示可配置可写，cp表壳可配置可写并且控制平台要用
>  options: 使用时请与给的配置列表保持一致；

####  获取设备的配置：

- 请求

```json
{
    "action": "config-get",
    "reqid":192839,         // 透传值，设备内唯一
    "params": [
        "relay-mode",
        "therm-mode",
        "algm-threshold"
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
        }
    ],
    "reqid":192839,         // 透传值，设备内唯一
    "retcode": 0
}
```

####  配置设备

- 请求

```json
{
    "action": "config-set",
    "reqid":192839,         // 透传值，设备内唯一
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
    "reqid":192839,         // 透传值，设备内唯一
    "retcode": 0
}
```

####  配置说明

以下为可配置数据与说明，实际使用时，同一类型的配置项最好一次下发，部分配置重启才能生效。对于多数场景，使用系统默认配置即可。

```json
{
    "items":[

    {
        "key":"org-id",
        "value":"Bashi Technology",
        "class":"interactive",
        "desc" : "组织，与GMT 0 相差的分钟数",
        "options" : "cp"
    },

    {
        "key":"door-id",
        "value":"Build1 Door2",
        "class":"interactive",
        "desc" : "当前门禁机管理的门的名称",
        "options" : "cp"
    },

    {
        "key":"show-mode",
        "value":"desc",
        "class":"interactive",
        "desc" : "显示模式，name - 仅姓名, desc - 姓名+部门, none - 不显示文字",
        "options" : "cp"
    },

    {
        "key":"device-mode",
        "value":"advt",
        "class":"interactive",
        "desc" : "设备模式，acs - 门禁模式，lock - 门锁模式，advt - 广告模式，therm - 测温模式",
        "options" : "cp"
    },

    {
        "key":"voice-mode",
        "value":"default",
        "class":"interactive",
        "desc" : "声音模式， disable - 无语音提示，default - 默认语音提示，user - 用户自定义",
        "options" : "cp"
    },

    {
        "key":"lang-mode",
        "value":"cn",
        "class":"interactive",
        "desc" : "语言配置，使用的文字、语音语言",
        "options" : "cp"
    },

    {
        "key":"timezone",
        "value":"-480",
        "class":"interactive",
        "desc" : "时区，与GMT 0 相差的分钟数",
        "options":"cp"
    },

    {
        "key":"ether-ipv4-address-mode",
        "value":"dynamic",
        "class":"network",
        "desc" : "以太网模式，dynamic - 动态获取IP，static - 静态IP地址模式",
        "options" : "wr"
    },

    {
        "key":"ether-ipv4-address",
        "value":"192.168.20.10",
        "class":"network",
        "desc" : "以太网地址，静态IP地址模式时配置的本机IP地址",
        "options" : "wr"
    },

    {
        "key":"ether-ipv4-mask",
        "value":"255.255.255.0",
        "class":"network",
        "desc" : "以太网掩码，静态IP地址模式时配置的本机IP地址掩码",
        "options" : "wr"
    },

    {
        "key":"ether-ipv4-gw",
        "value":"192.168.20.1",
        "class":"network",
        "desc" : "以太网网关，静态IP地址模式时配置的网关",
        "options" : "wr"
    },

    {
        "key":"ether-dns-server",
        "value":"114.114.114.114",
        "class":"network",
        "desc" : "以太网网关，静态IP地址模式时配置的DNS服务器",
        "options" : "wr"
    },

    {
        "key":"4g-mode",
        "value":"disable",
        "class":"network",
        "desc" : "4G模式，disable - 不支持，enable - 支持",
        "options" : "wr"
    },

    {
        "key":"ble-mode",
        "value":"none",
        "class":"network",
        "desc" : "蓝牙模式，none - 不支持，door - 远程开门， attendance - 考勤",
        "options" : "wr"
    },
    
    {
        "key":"wifi-mode",
        "value":"ap",
        "class":"network",
        "desc" : "WIFI模式，none 不使能wifi, ap - 热点模式，sta - 站点模式，mixed - 混合模式",
        "options" : "wr"
    },

    {
        "key":"wifiap-ssid",
        "value":"walos-wifi-abc",
        "class":"network",
        "desc" : "WIFI热点模式SSID，热点模式时配置",
        "options" : "wr"
    },

    {
        "key":"wifiap-passwd",
        "value":"1234567890",
        "class":"network",
        "desc" : "WIFI热点模式密码，热点模式时配置",
        "options" : "wr"
    },

    {
        "key":"wifista-ssid",
        "value":"walos-wifi-abc",
        "class":"network",
        "desc" : "WIFI站点模式时连接的热点SSID",
        "options" : "wr"
    },

    {
        "key":"wifista-passwd",
        "value":"1234567890",
        "class":"network",
        "desc" : "WIFI站点模式时连接的热点密码",
        "options" : "wr"
    },

    {
        "key":"manager-mode",
        "value":"cloud",
        "class":"manager",
        "desc" : "管理模式，cloud - 接入八识云，local - 接入门禁控制器， none - 仅提供web服务，wechat - 接入企业微信， client - HTTP客户端",
        "options" : "wr"
    },

    {
        "key":"manager-server-address",
        "value":"127.0.0.1",
        "class":"manager",
        "desc" : "管理服务器地址",
        "options" : "wr"
    },

    {
        "key":"manager-server-port",
        "value":"3000",
        "class":"manager",
        "desc" : "管理服务器端口",
        "options" : "wr"
    },

    {
        "key":"manager-server-path",
        "value":"report",
        "class":"manager",
        "desc" : "管理服务器路径",
        "options" : "wr"
    },

    {
        "key":"hearbeat-duration" ,
        "value": "5",
        "class":"manager",
        "desc" : "心跳间隔，三次心跳丢失会断开重连接",
        "options" : "wr"
    },
    
    {
        "key":"rs485-mode",
        "value":"none",
        "class":"peripherals",
        "desc" : "继电器模式， none - 不工作，door - 开门，voice - 语音输出， therm - 测温， user - 用户定义",
        "options" : "cp"
    },

    {
        "key":"relay-mode",
        "value":"enable",
        "class":"peripherals",
        "desc" : "继电器模式， disable - 继电器不工作，enable - 继电器工作",
        "options" : "cp"
    },

    {
        "key":"therm-mode",
        "value":"none",
        "class":"peripherals",
        "desc" : "测温模式， none - 不工作，wrist - 腕温，forehead - 额温",
        "options" : "cp"
    },

    {
        "key":"therm-fix",
        "value":"0",
        "class":"peripherals",
        "desc" : "测温修正值",
        "options":"cp"
    },

    {
        "key":"therm-normal",
        "value":"32.0",
        "class":"peripherals",
        "desc" : "测温最低有效值",
        "options":"cp"
    },

    {
        "key":"therm-alarm",
        "value":"37.5",
        "class":"peripherals",
        "desc" : "测温报警值",
        "options":"cp"
    },

    {
        "key":"alarm-door",
         "value":"disable",
         "class":"peripherals",
         "desc" : "报警输入-开门状态，disable - 不使能，enable - 使能",
         "options" : "cp"
    },

    {
        "key":"alarm-fire",
         "value":"disable",
         "class":"peripherals",
         "desc" : "报警输入-火警状态，disable - 不使能，enable - 使能",
         "options" : "cp"
    },

    {
        "key":"alarm-disassemble",
         "value":"disable",
         "class":"peripherals",
         "desc" : "报警输入-防拆状态，disable - 不使能，enable - 使能",
         "options" : "cp"
    },

    {
        "key":"alarm-push",
         "value":"disable",
         "class":"peripherals",
         "desc" : "报警输入-开门开关，disable - 不使能，enable - 使能",
         "options" : "cp"
    },

    {
        "key":"wigan",
         "value":"disable",
         "class":"peripherals",
         "desc" : "韦根口，disable - 不使能，enable - 使能",
         "options" : "cp"
    },

    {
        "key":"card-mode",
        "value":"none",
        "class":"peripherals",
        "desc" : "刷卡模式， none - 无刷卡，id - id卡， en - 加密卡， as - 非对称加密卡",
        "options": "cp"
    },

    {
        "key":"visitor-mode",
        "value":"none",
        "class":"peripherals",
        "desc" : "访客模式，none - 不支持，qrcode - 二维码读头， code - 密码输入， all - 都支持",
        "options" : "cp"
    },
    
    {
        "key":"volume-in",
        "value":"100",
        "class":"peripherals",
        "desc" : "声音输入，输入音频大小，1～100",
        "options" : "cp"
    },

    {
        "key":"volume-out",
        "value":"100",
        "class":"peripherals",
        "desc" : "声音输出，输出音频大小，1～100",
        "options" : "cp"
    },

    {
        "key":"light-rgb",
        "value":"100",
        "class":"peripherals",
        "desc" : "补光灯强弱，白光补光大小，1～100",
        "options" : "cp"
    },

    {
        "key":"light-ir",
        "value":"100",
        "class":"peripherals",
        "desc" : "补光灯强弱，红外补光大小，1～100",
        "options" : "cp"
    },

    {
        "key":"lcd-light",
        "value":"100",
        "class":"peripherals",
        "desc" : "屏幕背光亮度，1～100",
        "options" : "cp"
    },

    {
        "key":"auth-mode",
        "value":"single",
        "class":"algorithm",
        "desc" : "核验模式，single - 刷卡或人脸，card - 刷卡+人脸， therm - 人脸+测温",
        "options" : "cp"
    },

    {
        "key":"algm-threshold",
        "value":"0.5",
        "class":"algorithm",
        "desc" : "识别阈值，根据算法不同，此值不同",
        "options" : "cp"
    },

    {
        "key":"algm-alive",
        "value":"enable",
        "class":"algorithm",
        "desc" : "是否带活体检测，disable - 无活体检测， enable - 带活体检测",
        "options" : "cp"
    },

    {
        "key":"algm-face-size-min",
        "value":"120",
        "class":"algorithm",
        "desc" : "人脸像素数，最小值",
        "options" : "cp"
    },

    {
        "key":"algm-face-size-max",
        "value":"1080",
        "class":"algorithm",
        "desc" : "人脸像素数，最大值",
        "options" : "cp"
    },

    {
        "key":"algm-face-clear",
        "value":"92",
        "class":"algorithm",
        "desc" : "人脸清晰度阈值，用于人脸特征提取",
        "options" : "cp"
    },

    {
        "key":"algm-face-zone-x",
        "value":"0",
        "class":"algorithm",
        "desc" : "人脸识别与人脸检测区域范围，像素数。x,y,w,h",
        "options" : "cp"
    },

    {
        "key":"algm-face-zone-y",
        "value":"0",
        "class":"algorithm",
        "desc" : "人脸识别与人脸检测区域范围，像素数。x,y,w,h",
        "options" : "cp"
    },
    
    {
        "key":"algm-face-zone-w",
        "value":"1080",
        "class":"algorithm",
        "desc" : "人脸识别与人脸检测区域范围，像素数。x,y,w,h",
        "options" : "cp"
    },
    
    {
        "key":"algm-face-zone-h",
        "value":"1920",
        "class":"algorithm",
        "desc" : "人脸识别与人脸检测区域范围，像素数。x,y,w,h",
        "options" : "cp"
    },

    {
        "key":"algm-face-angle-yaw",
        "value":"45",
        "class":"algorithm",
        "desc" : "人脸角度， yaw - 偏航角(-90, 90)正左转；负右转",
        "options" : "cp"
    },

    {
        "key":"algm-face-angle-roll",
        "value":"45",
        "class":"algorithm",
        "desc" : "人脸角度， roll - 横滚角(-90, 90)正：左低右高；负：左高右低",
        "options" : "cp"
    },

    {
        "key":"algm-face-angle-pitch",
        "value":"30",
        "class":"algorithm",
        "desc" : "人脸角度， pitch - 仰角(-90, 90)正：仰视；负：俯视",
        "options" : "cp"
    }

    ]
}

```
