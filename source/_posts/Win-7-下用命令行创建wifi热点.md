title: "Win 7 下用命令行创建wifi热点"
date: 2015-04-20 20:31:43
tags: 
- wifi
categories:
- 配置
---
1. 以管理员身份运行命令提示符；
2. 设定虚拟WiFi网卡：

    在打开的窗口中运行命令：
``` bash
    netsh wlan set hostednetwork mode=allow ssid=name key=password
```
    此命令有三个参数：
 - mode：是否启用虚拟WiFi网卡，禁用:disallow。
 - ssid：wifi名称，最好用英文(以name为例)。
 - key：wifi密码，八个以上字符(以password为例)。
3. 启用虚拟WiFi网卡

    在打开的窗口中运行命令：
``` bash
    netsh wlan start hostednetwork
```
    开启成功后，网络连接中会多出一个网卡为“Microsoft Virtual WiFi Miniport Adapter”的无线连接2。
4. 设置Internet连接共享：

    在“网络连接”窗口中，右键单击已连接到Internet的网络连接，选择“属性”→“共享”，>勾上“允许其他······连接(N)”并选择“无线网络连接2”。

    确定之后，提供共享的网卡图标旁会出现“共享的”字样，表示“宽带连接”已共享至“无>线网络连接2”。
5. 关闭wifi:
``` bash
    netsh wlan stop hostednetwork
    netsh wlan set hostednetwork mode=disallow
```
