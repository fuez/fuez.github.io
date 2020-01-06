---
title: "nginx"
date: 2018-08-07T16:39:34+08:00
lastmod: 2019-06-20T18:02:24+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [How to add redirect/rewrite ruls](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

`return` is preferrable over `rewrite`:

```ini
server {
    listen 80;
    listen 443 ssl;
    ssl_certificate ssl/1_gezhitech.cn_bundle.crt;
    ssl_certificate_key ssl/2_gezhitech.cn.key;
    server_name gezhitech.cn;
    return 301  https://www.gezhitech.cn$request_uri;
}
```

For detail, refer to [official site doc](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

## Enable [nginx as a websocket proxy](https://www.nginx.com/blog/websocket-nginx/)

Add directives like the following:

```ini
location /wsapp/ {
    proxy_pass http://wsbackend;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}
```

## How to configure nginx with multiple locations with different root folders on subdomain?

You need to use the [`alias`](http://nginx.org/en/docs/http/ngx_http_core_module.html#alias) directive for other locations like this:

```ini
server{
    listen 8080;
    server_name _;

    location / {
        root /opt/nginx/;
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
    }

    location /kubernetes-release {
        alias /data/kubernetes-release;
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
    }
}
```


## How to configure a nginx file server?

Put some configure like the following to */etc/nginx/conf.d/*:

```ini
server{
    listen 8080;
    server_name _;
    root /opt/nginx/;

    location /{
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
    }
}
```
