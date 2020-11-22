# 版本信息

杭州八识科技产品版本有三种：

1. 硬件版本：标明产品硬件本身的版本；
2. 算法版本：标明使用的算法的版本；
3. WalOS版本：标明嵌入式软件版本；

本文指的是WalOS版本信息；

#### WalOS版本号采用 x.y.z 形式:

- x 代表大的架构变化或或重大的特性的添加，主要是协议层面的； 
- y 代表小特性增加的版本; 单数是linux版本， 双数是android版本;
- z 代表bugfix版本号，仅修改BUG，不添加特性；

#### linux版本为：

- v1.x 版本：630平台使用的版本，内存极度受限，一些接口做了取舍；
- v2.x 版本：WalOS平台版本，用于所有支持WalOS的设备，目前支持XM650，3516DV300，X2600等；

#### android版本：

- v2.x 版本：支持MT6737、MT6762、地平线X1600、RK3288等版本；


修改记录：

1. 2020-08-27
添加：
    // 识别后显示用户姓名方案
    // none - 不显示姓名， name - 仅显示姓名， desc - 显示姓名、部门。 默认desc
    {
        "key":"show-mode",
         "value":"desc",    
         "options" : "cp"
    },

    // 认证模式
    // single - 任何一种认证可开门， card - 先刷卡后识别人脸再开门， face - 先识别人脸再刷卡开门， therm - 先识别人脸再测温开门
    {
        "key":"auth-mode",
         "value":"single",
         "options" : "cp"
    },
    
    // 产品语言： cn - 中文， en - 英文， 支持的语言种类视硬件版本情况
    {
        "key":"lang-mode",
         "value":"cn",
         "options" : "wr"
    },

    // 是否支持语音播放：default: 支持，默认方式； disable：不支持；user: 定制用户定制方式（不使用）
    {
        "key":"voice-mode",
        "value":"default",
        "options" : "cp",
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

修改含义：
    // mixed 取消，local 连接本地软件/盒子， cloud连接云平台
    {
        "key":"manager-mode",
         "value":"mixed",
         "options" : "wr"
    },

    // lock, acs, advt 含义不变，取消therm模式，therm-mode配置非none后，acs与advt模式均可测温
    {
        "key":"device-mode",
         "value":"advt",
         "options" : "cp"
    },

删除：

    // 访客二维码UI显示时延
    {
        "key":"visitor-duration",
        "value": "16",
        "options" : "cp",
    },

    // 识别后显示时延
    {
        "key":"recognized-duration",
        "value": "2",
        "options" : "cp",
    },



