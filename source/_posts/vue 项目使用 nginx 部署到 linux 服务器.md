---
title: vue 项目使用 nginx 部署到 linux 服务器
date: 2022-04-02 16:05:23
tags:
  - 工具
  - nginx
  - vue
  - linux
categories:
  - [工具使用]
  - [前端]
---

![nginx](https://s2.loli.net/2022/05/31/BEuZd82ec3MVKy7.webp)

**vue** 通过 **nginx** 部署。 

<!-- more -->

1. 将打包好的文件放在 **nginx** 的 **html** 目录中

2. 修改 `conf/nginx.conf` 文件，新增项目的配置信息，多个项目可配置多个（指定不同的端口号和文件路径）

```conf
server {
  listen       443 ssl; # 443 端口号    ssl 开启HTTPS
  server_name  www.baidu.com; # 域名
  ssl_certificate /usr/local/nginx/cert/www.baidu.com.pem;     # 证书文件
  ssl_certificate_key /usr/local/nginx/cert/www.baidu.com.key; # 证书文件
  charset utf-8;

  location / {
    root   /usr/local/nginx/html/projectName; # 前端打包文件
    try_files $uri $uri/ /index.html;
    index  index.html index.htm;
  }

  location /prod-api/ { # 代理地址
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass https://localhost:8080/;  # 后端接口
  }

  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
    root   html;
  }
}
```

3. 在 **nginx** 目录下执行 `nginx -s reload` 重启服务

完成！
