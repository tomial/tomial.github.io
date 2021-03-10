---
title: UDP检验和
date: 2019-11-12 16:10:43

tags:
- Web

categories : 
- 计算机网络
---

## UDP检验和

UDP 检验和提供了差错检测的功能，可以让接收方检测所接收到的UDP报文段是否存在错误。但要注意，**UDP虽然提供了差错检验的功能，但对差错恢复无能为力。**

[RFC768]:(https://tools.ietf.org/html/rfc768)的定义：

> Checksum is the 16-bit one's complement of the one's complement sum of a pseudo header of information from the IP header, the UDP header, and the data,  padded  with zero octets  at the end (if  necessary)  to  make  a
> multiple of two octets.

UDP报文段的结构：

```
                  0      7 8     15 16    23 24    31
                 +--------+--------+--------+--------+
                 |          source address           |
                 +--------+--------+--------+--------+
                 |        destination address        |
                 +--------+--------+--------+--------+来自IP首部的
                 |  zero  |protocol|   UDP length    |伪首部字段
                 +--------+--------+--------+--------+----------
                 +--------+--------+--------+--------+
                 |     Source      |   Destination   |
                 |      Port       |      Port       |
                 +--------+--------+--------+--------+
                 |                 |                 |
                 |     Length      |    Checksum     |UDP首部字段
                 +--------+--------+--------+--------+----------
                 |      
                 |          data octets ...
                 +---------------- ...报文
```

从RFC768文档的定义中可以看到，检验和即**checksum**，是

1. 包含来自IP首部字段信息的伪首部字段
2. UDP首部字段
3. 数据

这些字段每16比特的反码之和所求得的反码，检验和长度为16比特

