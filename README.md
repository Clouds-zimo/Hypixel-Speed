[![Build and release](https://github.com/AllesUgo/Minecraft-Speed-Proxy/actions/workflows/release.yaml/badge.svg)](https://github.com/AllesUgo/Minecraft-Speed-Proxy/actions/workflows/release.yaml)![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/AllesUgo/Minecraft-Speed-Proxy)![GitHub all releases](https://img.shields.io/github/downloads/AllesUgo/Minecraft-Speed-Proxy/total)![GitHub](https://img.shields.io/github/license/AllesUgo/Minecraft-Speed-Proxy)![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/AllesUgo/Minecraft-Speed-Proxy)
# Hypixel-Speed

Minecraft加速IP程序  
能够代理Minecraft服务器，并拥有白名单、用户控制、流量展示、MOTD自定义等功能，支持IPv6，能够代理Forge客户端    
~~本项目使用C语言编写~~ 新版已改用C++，内存占用极低，在配置较低的服务器上拥有更好的表现  
改用C++编写，大幅度降低了崩溃的发生率，降低了内存泄露的可能并有了更好的项目结构  
新版已支持跨平台编译，支持Windows、Linux  

# 如何获取程序(Linux Ubuntu)，(Windows请参考Windows编译指南，或直接使用发行版)
### 通过源码获取
1.克隆仓库到你的Linux服务器上  
```bash
sudo apt update
sudo apt install -y git
git clone https://github.com/AllesUgo/Minecraft-Speed-Proxy.git
```
2.在Linux上安装linux c++编译工具包
    例如：
```bash
sudo apt update
sudo apt install -y g++ cmake make 
```
3.进入包含 `CMakeList.txt`的目录编译项目,编译完成后会在项目目录下生成可执行文件
```bash
cd Minecraft-Speed-Proxy
cmake -DCMAKE_BUILD_TYPE=Release .
cmake --build .
```

## 配置文件解释
程序除通过命令行参数设置启动外，还可以使用配置文件启动  
配置文件是一个JSON格式的文本文件，默认的配置文件可以通过`./Hypixel-Speed -c config.json`生成(请注意权限问题)  
其默认内容如下
```json
{
	"LocalIPv6":	false,
	"LocalAddress":	"0.0.0.0",
	"LocalPort":	25565,
	"RemoteIPv6":	false,
	"Address":	"mc.hypixel.net",
	"RemotePort":	25565,
	"MaxPlayer":	-1,
	"MotdPath":	"",
	"DefaultEnableWhitelist":	true,
	"WhiteBlcakListPath":	"./WhiteBlackList.json",
	"AllowInput":	true,
	"ShowOnlinePlayerNumber":	true,
	"LogDir":	"./logs",
	"ShowLogLevel":	0,
	"SaveLogLevel":	0
}
```  

各字段解释如下；

|键名|类型|键值|
|-|-|-|
|LocalIPv6|布尔|本机地址是否使用IPv6|
|LocalAddress|字符串|本机地址,一般填`0.0.0.0`,IPv6也可使用`::`|
|LocalPort|整数|本机端口|
|RemoteIPv6|布尔|远程服务器地址是否使用IPv6|
|Address|字符串|远程服务器地址，可以是域名或IP地址,如`mc.hypixel.net`|
|RemotePort|整数|远程服务器端口|
|MaxPlayer|整数|最大玩家数，-1为不限制|
|MotdPath|字符串|motd文件路径，为空则使用默认motd|
|DefaultEnableWhitelist|布尔|是否默认启用白名单|
|WhiteBlcakListPath|字符串|白名单黑名单文件路径|
|AllowInput|布尔|是否允许输入命令|
|ShowOnlinePlayerNumber|布尔|是否显示在线玩家数(当前版本暂时无效)|
|LogDir|字符串|日志文件目录|
|ShowLogLevel|整数|显示日志等级，-1为Debug日志，0为显示常规日志，1为显示警告及以上，2为显示错误及以上，3为显示玩家状态，一般填0|
|SaveLogLevel|整数|保存日志等级，-1为Debug日志，1为保存警告及以上，2为保存错误及以上，3为保存玩家状态，一般填0|

生成配置文件请使用
```bash
./Hypixel-Speed -a <配置文件路径>
```
使用配置文件启动请使用
```bash
./Hypixel-Speed -c <配置文件路径>
```
例如:
```bash
./Hypixel-Speed -c config.json
```
## 如何自定义motd
motd文件是一个文本文件，可以通过配置文件指定。若配置文件中的`MotdPath`字段为空则使用默认motd
以下是一个motd文件的例子
```JSON
{
    "version": {
        "name": "1.8.7",
        "protocol": 47
    },
    "players": {
        "max": 100,
        "online": 5,
        "sample": [
            {
                "name": "thinkofdeath",
                "id": "4566e69f-c907-48ee-8d71-d7ba5aa00d20"
            }
        ]
    },    
    "description": {
        "text": "Hello world"
    },
    "favicon": "data:image/png;base64,<data>"
}
```
MOTD必须是一个JSON格式的文本文件，其中的字段均可空或部分为空，当某字段为空时，服务器将会使用默认值填充该字段  
例如，当players字段为空时，服务器将会使用设置的最大玩家数、在线玩家数和玩家列表填充该字段  
若希望获取现有服务器的motd数据，可以使用命令`./Hypixel-Speed --get-motd`并按照提示操作
