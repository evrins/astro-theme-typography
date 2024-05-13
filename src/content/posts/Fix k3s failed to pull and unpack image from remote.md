---
title: Fix k3s failed to pull and unpack image from remote
pubDate: 2024-04-15 13:56:44
categories: [K3S]
description: ''
---
 
昨天晚上开始 k3s 从 ghcr 上拉取镜像失败, 显示错误信息 `PullImage from image service failed" err="rpc error: code = Unknown desc = failed to pull and unpack image `, 但是本地使用 docker pull 可以正常拉取镜像. 判断应该是网络代理的问题.
在 k3s 官网上有这样一段说明 k3s [链接](https://docs.k3s.io/advanced#configuring-an-http-proxy) 安装的时候会读取当前 session 中和代理相关的环境变量并将他们保存在 `/etc/systemd/system/k3s.service.env` 和 `/etc/systemd/system/k3s-agent.service.env` 这两个文件中, 这样后续 k3s 启动的时候会使用这些代理配置. 由于国内的网络环境问题, 安装时使用了代理, 而使用的代理服务正好流量耗尽了, 导致拉取镜像失败. 编辑了 `/etc/systemd/system/k3s.service.env` 文件删除了代理配置, 重启 k3s 服务之后就正常了.
