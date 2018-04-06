---
title: Linux 端口流量统计
date: 2016-10-06
---
**获取80端口上的流量,iptables的规则如下:**
```shell
iptables -A INPUT -p tcp --dport 80
```
**获取该端口的字节数**
```shell
iptables -vnxL INPUT|awk '/tcp.*80/{print $2}'
```
***注：如果是网关的话，把INPUT 改为FORWARD***库")