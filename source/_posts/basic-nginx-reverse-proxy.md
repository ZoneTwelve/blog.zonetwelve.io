---
title: basic Nginx Reverse Proxy
date: 2021-08-21 16:16:10
tags:
  - nginx
  - reverse proxy
  - http
  - port forwarding
---


# Intro
服務開發久了，自然就會想做一些無聊的事情，然而我發現我想開一個以上的服務就會遇到 Port 衝突的問題，所以我就透過 VM + Nginx 的 Port forwarding 實現多個服務的營運。

<!-- more -->

# Network structure
接下來的步驟可能都會非常抽象，所以我先提供一個思維導圖讓各位可以理解接下來的作業是如何進行的。

## 架構圖
![Network structure](nginx-reverse-proxy_Network-structure_B.png)

## 流程
我們分別有 Client A/B/C 要請求到我們開發的服務 `A.zonetwelve.io / B.zt.app / B.zonetwelve.io`，但是我們只有一個 IP 以及一個 80/443 Port，因此需要透過一個管理的服務去負責 redirect，所以 Nginx 在這裡就是扮演這個角色， Reverse Proxy (10.0.1.2) 是我們的 Proxy，讓 Sever 都將流量導到 10.0.1.2，然後讓 10.0.1.2 透過 `proxy_pass` 去做 forwarding 的動作。

我們先建立一個基本的設定(參考用)

所以當我請求 A.zonetwelve.io ，服務就會被導到 10.0.1.10 的 80 Port
目前以上設定會這樣導向
- A.zonetwelve.io
  - 140.115.1.100
    - 10.0.1.10
- B.zt.app
  - 140.115.1.100
    - example.com:8080
- B.zonetwelve.io
  - 140.115.1.100
    - 10.0.1.11


```
server {
  listen 80;
  listen [::]:80;

  # IoT Center
  server_name A.zonetwelve.io;

  location / { 
    proxy_pass http://10.0.1.10/;
  }
}

server {
  listen 80;
  listen [::]:80;

  server_name B.zt.app;

  location / {
    proxy_pass http://example.com:8080/;
  }
}

server {
  listen 80;
  listen [::]:80;

  server_name B.zonetwelve.io

  location / {
    proxy_pass http://10.0.1.11/;
  }
}
```
<small>未特別說明的 http 都是走 80</small>

# Nginx
## Server name (Service Path)
我們知道 nginx 有可供 `server_name` 讓我們可以利用 `regular expression` 去將特定 sub-domain 導到我們的服務上。
等一下會配合 `proxy_pass` 的方式將連線 redirect 到我們的目的伺服器。
有什麼其他需求可以直接上 [NGINX Server Name](http://nginx.org/en/docs/http/server_names.html) 的文件查詢

- /etc/nginx/conf.d/default.conf
```
server {
    listen       80;
    server_name  example.test  www.example.test;
    ...
}
```


### 延伸： /eth/hosts
各位可能還沒有自己的 domain，所以可以透過修改 `/etc/hosts`，就可以把 DNS 查詢的 domain 導到自己的 host 上。
```
127.0.0.1 localhost
127.0.1.1 example.test
```

---
---

## Proxy pass (服務 Redirect)
我先在我的環境當中架設多個虛擬機 (include Reverser Proxy)，並且設定好相關的網路即可進行下一步。
不熟悉的同學，可以利用 VMWare 架設 Ubuntu Desktop/Server 即可，只要可以讓 A VM 可以 ping 到 B VM 就好了。
[NGINX Reverse Proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

- /etc/nginx/conf.d/default.conf
```
server {
  listen 80;
  listen [::]:80;
  server_name example.test;

  location / {
    proxy_pass https://10.0.1.10/;
  }
}
```

## upstream (Load balancing)
在一般的服務當中，如果你有一個電腦叢集( Computing Cluster )，你就會知道 `Load balancing` ，因為 Cluster 的目的之一就是解決這個問題，所以 [`upstream`](http://nginx.org/en/docs/http/ngx_http_upstream_module.html) 就可以替我們 handle 相關的問題。
這次我順便沿用上次開發的 [My first smart lamp](/tags/my-first-smart-lamp/) 當作要處理的服務
<small>因為我現在沒有處理好 Raspberry pi Cluster，所以只設定 VM 作為 Load balancing，未來只要刪除註解就可以用了。</small>
- /etc/nginx/conf.d/default.conf 
```
upstream iot_center_service {
  server 10.0.1.200:54088 weight=3; # VM IP Address
  #server 10.0.2.2:54088; # It's a fake static IP for future using
  #server 10.0.2.5:54088; # The same
}

server {
  listen 80;
  listen [::]:80;

  server_name example.test;

  proxy_set_header Host $http_host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  location / {
    proxy_pass http://iot_center_service;
  }
}
```

## Nginx Secure Connection (HTTPS)
開發 Web 的朋U們應該都會知道 SSL/TLS 的存在，就是一個提供網站安全連線的協議 即 HTTPS (HyperText Transfer Protocol Secure)，想了解更多的可以自己搜尋相關文獻。
簡單來說，讓我們的網站跟使用者建立安全的連線，所以在 Nginx 可以先建立安全連線，這樣就算轉到內部網路的時候是不安全的協議也可以一定程度的保護使用者。
那就直接上設定
相關文件: 
 - [HTTPS Server](http://nginx.org/en/docs/http/configuring_https_servers.html)
 - [SSL Modules](http://nginx.org/en/docs/http/ngx_http_ssl_module.html)

- /etc/nginx/conf.d/default.conf
```
server {
  listen 443 ssl;
  listen [::]:443 ssl;

  ssl_certificate /etc/nginx/ssl/app_1_ssl.crt;
  ssl_certificate_key /etc/nginx/ssl/app_1_ssl.key;

  server_name example.test;

  proxy_set_header Host $http_host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  location / {
    proxy_pass https://iot_center_service;
  }
}

```
<small>注意！我內容都是有修改的，例如說 `listen` 的 `port` 跟 `location` 做的 `proxy_pass` 是導向到 https</small>


# 最終成果 Final result
這是我最終寫出來的設定，目前正在使用 w

- /etc/nginx/conf.d/default.conf
```
upstream iot_center_service {
    server 10.0.1.20:54088;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;

    ssl_certificate /etc/nginx/ssl/app_1_ssl.crt;
    ssl_certificate_key /etc/nginx/ssl/app_1_ssl.key;

    server_name A.zonetwelve.io;

    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    location / {
      proxy_pass https://iot_ecnter_service/;
    }
}
```

套用完設定以後可以使用 `nginx -s reload` 進行重新載入設定

這樣就成功完成 Reverse Proxy 啦
