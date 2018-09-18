---
title: 生成SSL证书
date: 2018-03-29 18:12:33
tags: [SSL]
categories: HTTPS
---

今天老大让我测试https的网站是否可以绕过备案，正常访问。所以需要生成SSL证书。理论上我们自己可以签发SSL安全证书，但是我们自己签发的安全证书不会被主流浏览器信任，所以我们需要被信任的证书授权中心（CA）签发的安全证书。 我用的是 Let's Encrypt的，生成方便，但是证书有90天的有效期，自己测试用，也非常适合个人使用。

<!-- more -->

`Certbot` 是Let's Encrypt官方推荐的获取证书的客户端，可以帮我们获取免费的Let's Encrypt 证书。
以下是我的Ngix + Ubuntu 16.04 为例的安装教程。

 apt-get 包
 ```
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx 
 ```
安装 Nginx 插件
```
$ sudo certbot --nginx
```
运行
```
$ sudo certbot --nginx certonly
```

运行这个命令将获得一个证书，并且会自动编辑你的Nginx配置。不需要手动。

<center><img src="http://ofstpx613.bkt.clouddn.com/1522314525.jpg" width="600" ></center>


自动修改Nginx配置后如图：
如图我的配置
<center><img src="http://ofstpx613.bkt.clouddn.com/15223155421.jpg" width="600" ></center>


运行
```
$ sudo certbot renew --dry-run
```
因为我们加密证书有效期只有90天,此命令可以自动更新证书

---- 

<center><img src="http://ofstpx613.bkt.clouddn.com/dog_dribbble.gif" width="400" ></center>
