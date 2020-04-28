---
title: 内核插件 - V2Ray 集成
---

# 插件 - V2Ray 集成

- V2Ray 是如何和 ShadowsocksR、Trojan 等内核协同运作的

## 何为 V2Ray 集成

Qv2ray 特有功能，将插件内核与 V2Ray 核心进行整合。

许多内核没有内置分流引擎，例如 ShadowsocksR 等。在其他的一些实现中，这些内核通常依赖 PAC 功能进行分流。Qv2ray 正准备打破这种现状，用 V2Ray 核心来替换 PAC，从而实现更为统一的分流功能。

## 为何不用 PAC？

PAC 功能已被 Qv2ray 开发者弃用。我们不建议普通用户使用，下面是原因：

- PAC 依赖于现有的规则，例如可以用 `GFWList` 或其他域名列表进行生成。
- 手动编辑、调整 PAC 文件相当困难，拼写和语法错误往往是致命的。
- V2Ray 本身自带路由表实现以及来自官方社区活跃维护的 `geoip`、`geosite` 数据库。
- 使用 V2Ray 集成功能要比折腾 PAC 要容易得多。

## V2Ray 集成如何运作？

细节如下:

1. Qv2ray 检测到一个/某些出站配置属于一个/某些插件核心。
2. Qv2ray 为每个核心分配 HTTP/SOCKS5 入站端口。
3. Qv2ray 将插件核心的出站替换为对应核心的 HTTP/SOCKS5 入站端口，并自动进行匹配。
4. Qv2ray 向 V2Ray 配置文件中写入对应的路由表项，之后将对应的入站通过规则分流后，再导向插件的入站。
5. Qv2ray 首先根据配置启动所有的插件核心，最后再启动 V2Ray 核心。
6. Qv2ray 将直接从 V2Ray 核心处收集统计数据，而不需要从各自的插件中收集。

## V2Ray 集成的益处

- 绕过中国大陆域名与 IP 地址。
- 绕过局域网地址。
- 能够使用 Qv2ray 高级路由矩阵设定。
- 能够使用自定义的域名服务器（DNS）设定。