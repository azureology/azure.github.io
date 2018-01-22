---
title: 树莓派ZeroW入坑指南
date: 2017-12-23 10:48:20
tags: EmbDev
comments: true
---
# 效果图  
先上一张Raspberry Pi Zero W 原貌

![zeroW](https://ws1.sinaimg.cn/large/006tKfTcly1fmqinc7cy5j3160160gqs.jpg)

# 起源
说起创客不得不提到开源硬件Raspberry Pi(树莓派)。它是一款基于ARM的微型电脑主板，以MicroSD卡为硬盘，提供HDMI和USB等外部接口，可连接显示器和键鼠。以上部件全部整合在一张仅比信用卡稍大的主板上，具备所有PC的基本功能只需接通显示器和键盘，就能执行如电子表格、文字处理、玩游戏、播放高清视频等诸多功能。二百出头的价格，有没有长草?

![](https://ws3.sinaimg.cn/large/006tKfTcly1fmqiqkpbdpj30k00b6jsb.jpg)

前不久忙完课题的我入手了树莓派3B拔草，后期有了Windows10 IoT系统加持感觉潜力无穷！但试用几天下来3B给我的感觉还是体积太大，发热明显。经过一番搜索，树莓派Zero w进入视线！

![](https://ws3.sinaimg.cn/large/006tKfTcly1fmqiu614ojj30k00cigmv.jpg)

# 配置参数
树莓派Zero w是树莓派基金会今年二月底为庆祝其第五个生日发布的特别版，本质上是树莓派Zero加Wi-Fi蓝牙。树莓派Zero W售价10美元，使用与3B相同的Cypress CYW43438无线芯片，提供802.11n无线局域网和蓝牙4.1连接。

![](https://ws4.sinaimg.cn/large/006tKfTcly1fmqivhdrp3g30k00b8kjl.gif)

树莓派Zero W完整配置如下：1GHz单核CPU，512MB内存，mini-HDMI接口，Micro-USB OTG端口，Micro-USB电源接口，HAT兼容40针GPIO扩展口，复合视频和重置接头，CSI摄像机连接器，802.11n WiFi+蓝牙4.1（BLE）。实测满载电流160mA核心温度低，非常适合电池供电。少废话，开箱！

# 硬件准备
![](https://ws1.sinaimg.cn/large/006tKfTcly1fmqiyelkolj30k00k0gnd.jpg)

这是我在某宝定制的树莓派Zero w三件套，包含主板、亚克力外壳、散热片、HDMI转换器、OTG线、GPIO排针。其中排针需要自己焊接，自备TF卡刷入系统即可使用。

![](https://ws2.sinaimg.cn/large/006tKfTcly1fmqj046q6bj30k00f0gn1.jpg)

不得不说，Zero w真的小，长6cm的主板涵盖各种功能必须精工细作。值得一提的是HDMI接口右侧的梯形区域为Zero w的WiFi/BT天线，由瑞典天线大师Proant AB亲自操刀设计。天线位置处所有6层铜箔中都做了镂空处理，整成有一个梯形的“孔状”，而那个成形的部分被通孔（Vias）和几颗微小的电容器包围，构成了2.4GHz的谐振腔，通过电容器驱动，整个设计就组成了Pi Zero W的天线，堪称点睛之笔。为了方便后期连接uart串口和I2C接口我打算焊接GPIO排针。

![](https://ws1.sinaimg.cn/large/006tKfTcly1fmqj0gyw50j30k00f0jsg.jpg)

渣渣焊工，就不要吐槽了……为了保护爱板，我额外安装了散热片和亚克力支架，安装过程比较友好。整体外观如图：

![](https://ws3.sinaimg.cn/large/006tKfTcly1fmqj19lbtcj30k00k0wga.jpg)

# 系统安装
组装环节告一段落，由于Zero w使用了非标准接口，想要耍起来必须上转接器，那么日后工作中的小派可能会是这样：

![](https://ws3.sinaimg.cn/large/006tKfTcly1fmqj2kmxgfj30k00k0gn3.jpg)

这有悖Zero w设计的初衷，而且一点也不简洁，差评！抑或手头上没有HDMI显示器的同学，有啥办法可以只用一根线玩转Zero w？有，使用VNC或ssh远程登陆。

![](https://ws1.sinaimg.cn/large/006tKfTcly1fmqj2tgpvij30dc072t8s.jpg)

上树莓派官网下载Raspbian的系统镜像,受制于Zero w的性能我选择了不带图形界面的Lite版。若使用Desktop版可开启VNC进行连接。

![](https://ws4.sinaimg.cn/large/006tKfTcly1fmqj33pd6tj30bp05y74e.jpg)

下载完成使用Win32DiskImager烧写软件将文件写入SD卡，站起来走走喝杯茶，整个过程大约10分钟。好，重点来了！如何在没有屏幕的情况下配置WiFi连接并开启系统的ssh？ 

# 网络配置
首先在SD卡boot分区中新建wap_supplicant.conf文件，然后使用Notepad++写入如图内容。

```
#This is /boot/wpa_supplicant.conf

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN

network={
    ssid="WiFi名称1"
    psk="WiFi密码1"
    key_mgmt=WPA-PSK
    priority=5
}

network={
    ssid="WiFi名称2"
    psk="WiFi密码2"
    key_mgmt=WPA-PSK
    priority=4
}
```

可加入多个netwoek，其priority为优先级，大值优先，不能为负。注意保存时转换为Unix（LF）格式。最后再建立一个名为ssh的空白文件即可开启ssh功能。

# SSH连接
插电，启动！打开putty设置ssh参数，默认主机名raspberrypi，端口22，用户名pi，初始密码raspberry。

![](https://ws4.sinaimg.cn/large/006tKfTcly1fmqjbh7huej30co0c7jrz.jpg)

万事俱备，开始你的树莓派学习之旅吧！

![](https://ws4.sinaimg.cn/large/006tKfTcly1fmqjeb0oldj30hs0b7dga.jpg)

# 尾声
最后放张ZeroW和3B的对比图结题

![](https://ws4.sinaimg.cn/large/006tKfTcly1fmqjehnu91j30k00f0ta7.jpg)

Single core power !!!

![打赏二维码](https://ws4.sinaimg.cn/large/006tKfTcly1fmqjh9e3hzj30en0403yw.jpg)