---
layout: post
title: 基于vultr vps搭建SS服务
description: 基于vultr vps搭建SS服务服务
tags:
  - vps
  - vultr
  - shadow socks
published: true
category: blog
---
欢迎转发，请注明原文地址：http://bohai.github.io。
微博：@寻觅神迹

由于工作需要翻墙查阅各种资料，之前一直使用第三方提供的商业 SS 服务翻墙。
虽然价格便宜，每月几块钱。但是速度不尽如人意，经常半天打不开网页，更别说浏览 youtube 了。
还是自己动手，丰衣足食的好。

我选的是 vultr 这个国外的 vps 提供商，他家最便宜的套餐是 2.5 美元每月。
每月 500GB 流量，虚拟机是基于 KVM 的，每个虚拟机有 1 核，512MB 内存，20G SSD 存储。
在上边跑一个 SS 服务绰绰有余。

<a href="https://www.vultr.com/?ref=7220409"><img src="https://www.vultr.com/media/banner_1.png" width="728" height="90"></a>

>> 请务必使用这个链接，这样你我都会被赠送10美元。

vultr 还有一个值得称赞的地方，它支持使用支付宝。这样你就不需要使用国际信用卡或者 paypal 。
虚拟机的创建非常简单，这里就不多说了。至于机房的选择，一般建议是 losangel ，但是最近这个机房一直没有资源。
我选了 newyork 的机房，实际使用速度还是很满意的。基本上大部分时间，youtube 使用 720P 可以流畅观看。
网页秒开无压力。

vultr 还免费支持对虚拟机进行快照，这样你需要恢复的时候可以很快的进行恢复。 
对虚拟机内进行 SS 安装配置，可以使用 SSH 客户端进行连接配置。比较常用的 Putty 啦。
创建虚拟机过程比较简单，建议操作系统选择 centos7 。

搭建 ShadowSocks
===
推荐看秋水逸冰的《Shadowsocks 一键安装脚本（四合一）》的安装教程。

运行以下命令:
``` shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

选择脚本（Python、R、Go、libev），任选一个，建议选择 Go ：

``` shell
Which Shadowsocks server you'd select:
1.Shadowsocks-Python
2.ShadowsocksR
3.Shadowsocks-Go
4.Shadowsocks-libev
Please enter a number (default 1):
```

笔者选择Shadowsocks-Go，输入3......然后，输入密码和端口，笔者直接回车用默认：
``` shell
You choose = Shadowsocks-Go

Please enter password for Shadowsocks-Go
(default password: teddysun.com):

password = teddysun.com

Please enter a port for Shadowsocks-Go [1-65535]
(default port: 8989):

port = 8989


Press any key to start...or Press Ctrl+C to cancel
```

安装成功后，命令行出现：
``` shell
Congratulations, Shadowsocks-Go server install completed!
Your Server IP        :  45.32.73.59
Your Server Port      :  8989
Your Password         :  teddysun.com
Your Encryption Method:  aes-256-cfb

Welcome to visit: https://teddysun.com/486.html
Enjoy it!
```

TCP BBR 拥塞控制算法
===
TCP BBR算法是Google搞出来的玩意，没想到在翻墙上使用，效果出奇的好。
整个网络的速度可以提高很多。

使用root用户登录，运行以下命令：
``` shell
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
```

安装完成后，脚本会提示需要重启 VPS，输入 y 并回车后重启。重启后，执行命令：
``` shell
lsmod | grep bbr
```

TCP BBR要求Linux内核4.10以上。

参考：
https://www.diycode.cc/topics/738



[寻觅神迹]:    http://bohai.github.io  "寻觅神迹"
