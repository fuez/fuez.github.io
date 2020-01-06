---
title: "devops"
date: 2019-06-08T10:36:33+08:00
lastmod: 2020-01-03T08:48:24Z
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## 如何重置Nexus3的密码

主要步骤如下：

1、停服

2、进入OrientDB控制台：java -jar /usr/local/nexus/lib/support/nexus-orient-console.jar 

3、在控制台执行：connect plocal:/home/xiaoban/nexus-repository/nexus3/db/security admin admin

4、重置密码为admin123 ：

update user SET password="$shiro1$SHA-512$1024$NE+wqQq/TmjZMvfI7ENh/g==$V4yPw8T64UQ6GfJfxYq2hLsVrBY8D1v+bktfOxGdt4b/9BthpWPNUy/CBk6V9iA0nHpzYzJFWO8v/tZFtES8CA==" UPSERT WHERE id="admin"
5、启动服务


## [使用 Certbot + Let's Encrypt 小步快跑的奔向 HTTPS](https://learnku.com/laravel/t/2525/using-certbot-lets-encrypt-small-step-run-towards-https)

[Install on centos](https://certbot.eff.org/lets-encrypt/centosrhel7-nginx)
*Revised*: should install certbot on python3, like this

```sh
sudo python3 -m venv ~/py3
source ~/py3/bin/activate
pip install certbot certbot-nginx
sudo certbot --nginx  
```

如果状态显示，其中一些API server down了：https://letsencrypt.status.io/，可以通过`--server`命令行参数选择其他server， 详见: https://github.com/certbot/certbot/blob/master/docs/using.rst

## [acme.sh](https://github.com/Neilpang/acme.sh)

`curl https://get.acme.sh | sh` to install
<p>`acme.sh --issue -d hub.gezhitech.cn -w /usr/share/nginx/html` to get a new cert
<p>`mkdir -p /etc/nginx/ssl/hub.gezhitech.cn/ && acme.sh --install-cert -d  hub.gezhitech.cn --key-file /etc/nginx/ssl/hub.gezhitech.cn/key.pem --fullchain-file /etc/nginx/ssl/hub.gezhitech.cn/fullchain.pem` to install cert for nginx usage.

## Script to clean up not-used k8s namespaces

First, list all namespaces with helm chart deployed: `helm list | tail +2 | awk -F '\t' '{print $7}' > /tmp/using.txt`, also add system namespaces to this file, 'echo -e "default\nkube-public\nkube-system" >> /tmp/using.txt'
Then, list all existing  namespace: `k get ns | tail +2 | awk '{print $3} > /tmp/all.txt'
Last, using python script to get diff:

```python
a = set(x.strip() for x in open("/tmp/using.txt").read().splitlines())
b = [ x.strip() for x in open("/tmp/all.txt").read().splitlines()]
c = [x for x in b if x not in a]
with open("/tmp/diff.txt", "wt") as f:
  f.write("\n".join(c))

```

Finally, double check whether it is OK to delele those namespaces: `for n in `cat /tmp/diff.txt`; do echo $n; k -n $n get all ; done`, and if OK, delete all `for n in `cat /tmp/diff.txt`; do k delete ns $n ; done`

## Good series of articles explains [oauth2](https://medium.com/google-cloud/understanding-oauth2-and-building-a-basic-authorization-server-of-your-own-a-beginners-guide-cf7451a16f66)

## [Building a Basic Authorization Server using Authorization Code Flow](https://medium.com/@ratrosy/building-a-basic-authorization-server-using-authorization-code-flow-c06866859fb1)

## Nice `curl` example to POST data
>Learn how to use `EOF` to input json data

```sh
curl -i -X POST https://api.github.com/user/repos \
-H "Authorization: token <OAuth token>" \
-d @- << EOF
{
  "name": "blog",
  "auto_init": true,
  "private": true,
  "gitignore_template": "nanoc"
}
EOF
```

## Get `http_code` from `curl`

```sh
curl -s -o /dev/null -w "%{http_code}" http://www.example.org/
```

## Linux cpu/memory/fileio benchmark tool:[sysbench](https://www.howtoforge.com/how-to-benchmark-your-system-cpu-file-io-mysql-with-sysbench)

Install it on centos: `yum install -y sysbench` when `epel-release` is enabled

Benchmark fileio:

```sh
sysbench --test=fileio --file-total-size=2G prepare
sysbench --test=fileio --file-total-size=2G --file-test-mode=rndrw  --max-time=300 --max-requests=0 run
```

## [Apache Traffic Server](https://docs.trafficserver.apache.org/en/8.0.x/getting-started/index.en.html) can be used as both reverse proxy as well as forward proxy, it performs better than Squid or Varnish

A full conf example for reserse proxy with cache enabled is:

```ini
records.config:

CONFIG proxy.config.http.cache.http INT 1
CONFIG proxy.config.reverse_proxy.enabled INT 1
CONFIG proxy.config.url_remap.remap_required INT 1
CONFIG proxy.config.url_remap.pristine_host_hdr INT 1
CONFIG proxy.config.http.server_ports STRING 80 80:ipv6

remap.config:

map http://www.acme.com/api/ http://api-origin.acme.com/
map https://www.acme.com/api/ https://api-origin.acme.com/
map http://www.acme.com/ http://localhost:8080/
map https://www.acme.com/ http://localhost:8080/
map http://static.acme.com/ http://origin-static.acme.com/
map https://static.acme.com/ https://origin-static.acme.com/

storage.config:

/cache/trafficserver 500G

ssl_multicert.config:

ssl_cert_name=/path/to/secret/acme.rsa
```

## [Understand difference between reverse proxy, forward proxy and transparent proxy](https://docs.trafficserver.apache.org/en/8.0.x/getting-started/index.en.html)
>A reverse proxy appears to outside users as if it were the origin server, though it does not generate the content itself. Instead, it intercepts the requests and, based on the configured rules and contents of its cache, will either serve a cached copy of the requested content itself, or forward the request to the origin server, possibly caching the content returned for use with future requests.
> A forward proxy brokers access to external resources, intercepting all matching outbound traffic from a network. Forward proxies may be used to speed up external access for locations with slow connections (by caching the external resources and using those cached copies to service requests directly in the future), or may be used to restrict or monitor external access.
>A transparent proxy may be either a reverse or forward proxy (though nearly all reverse proxies are deployed transparently), the defining feature being the use of network routing to send requests through the proxy without any need for clients to configure themselves to do so, and often without the ability for those clients to bypass the proxy.

From my understanding, reverse proxy is often used at the server side to balance load.

## [Enable external access to graylog](http://docs.graylog.org/en/3.0/pages/configuration/web_interface.html)

## [Deploy gogs docker](https://garthwaite.org/docker-gogs.html)

Here is its [cheat sheet for configuration](https://gogs.io/docs/advanced/configuration_cheat_sheet)

## Aliyun mirror site: https://opsx.alibaba.com/mirror

## How to Disable Password Authentication for SSH?
Edit */etc/ssh/sshd_config* file, to make sure the following configurations are configured as below:
```
ChallengeResponseAuthentication no
PasswordAuthentication no
UsePAM no
```
And then restart *sshd* service.
## [How to install shadowsocks server](https://www.linode.com/docs/networking/create-a-socks5-proxy-server-with-shadowsocks-on-ubuntu-and-centos7)

On Ubuntu server:
```
apt-get update && apt-get upgrade -yuf
apt-get install -y --no-install-recommends gettext build-essential autoconf libtool libpcre3-dev asciidoc xmlto libev-dev libudns-dev automake libmbedtls-dev libsodium-dev git python-m2crypto
apt-get install -y libc-ares-dev
./autogen.sh
./configure
make && make install

```
For client on OSX, it  is called shadowsocksX, which could be downloaded from [github](https://github.com/shadowsocks/ShadowsocksX-NG/releases)


## Basic setup for influxdb and chronograf:
```
version: '2'
services:
  influx:
    container_name: influxdb
    image: influxdb:1.3
    environment:
      - INFLUXDB_DATA_QUERY_LOG_ENABLED=false
      - INFLUXDB_REPORTING_DISABLED=true
    ports:
      - "8086:8086"

  chronograf:
    container_name: chrono
    image: chronograf:1.3
    environment:
      - INFLUXDB_URL=http://influxdb:8086
    ports:
      - "8888:8888"
```
## How to list files from remote nginx server with index on?
Like this:`curl -sSL  http://pp03:8080/base_image/rpms | perl -n -e'/href="([a-zA-Z][^"]*)"/ && print $1, print "\n"'`
## [HowTo : Create CSR using OpenSSL Without Prompt (Non-Interactive](http://www.shellhacks.com/en/HowTo-Create-CSR-using-OpenSSL-Without-Prompt-Non-Interactive)
  + An example request to generate both key and CSR:`openssl req -nodes -newkey rsa:2048 -keyout example.key -out example.csr -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"`
  + An example request to generate CSR wth existing key:`openssl req -new -key example.key -out example.csr -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"`
=======
## Leverage [Let's Encrypt](https://letsencrypt.org/how-it-works/) to set up a trusted HTTPS server

## How to configure nginx for file server?
Just add a new conf file under _/etc/nginx/conf.d/_, like the following and restart nginx service:

```
server{
    listen 8080;
    server_name _;
    root /data/;

    location /{
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
    }
}
```

## Installations
### Install and configure lighttpd on centos/rhel
## Install by yum: `yum install -y lighttpd`
## Refer to [Configuration files for lighttpd](https://redmine.lighttpd.net/projects/1/wiki/docs_configuration) for detail configuration explaination
## Create a director such as _/data/www_ as document root, and update __server.document-root__ value
## Start lighttpd service: `systemctl start lighttpd`
## To enable uploading, refer to [how to enable cgi with perl](http://www.sitepoint.com/uploading-files-cgi-perl-2/)

### HAProxy
## [How to ue HAProxy to set up http load balance](https://www.digitalocean.com/community/tutorials/how-to-use-haproxy-to-set-up-http-load-balancing-on-an-ubuntu-vps)

## [Load balance with HAProxy](https://serversforhackers.com/load-balancing-with-haproxy)
## [Howto setup High-Available HAProxy with Keepalived](https://blog.laimbock.com/2014/10/01/howto-setup-high-available-haproxy-with-keepalived/)
A sample haproxy.cfg:
```
global
    log         127.0.0.1   local0
    log         127.0.0.1   local1 notice
    maxconn     4096
    user        haproxy
    group       haproxy
    nbproc      1
    pidfile     /var/run/haproxy.pid
 
defaults
    log         global
    option      tcplog
    option      dontlognull
    retries     3
    maxconn     4096
    option      redispatch
    timeout     connect 50000ms
    timeout     client  50000ms
    timeout     server  50000ms
 
listen mariadb-galera-writes
    bind 0.0.0.0:3307
    mode tcp
    option mysql-check user haproxy
    server db1 10.0.0.9:3306 check
    server db2 10.0.0.11:3306 check backup
    server db3 10.0.0.13:3306 check backup
 
listen mariadb-galera-reads
    bind 0.0.0.0:3306
    mode tcp
    balance leastconn
    option mysql-check user haproxy
    server db1 10.0.0.9:3306 check
    server db2 10.0.0.11:3306 check
    server db3 10.0.0.13:3306 check
 
# HAProxy web ui
listen stats 0.0.0.0:9000
    mode http
    stats enable
    stats uri /haproxy
    stats realm HAProxy\ Statistics
    stats auth haproxy:haproxy
    stats admin if TRUE
EOF
```

### Install filebeat for ELK stack
## [filebeat configuration detail](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-configuration-details.html)

### Setup CI environment on Ubuntu
## Install docker: `sudo apt-get install -y docker.io`
## Test docker installation: `sudo docker run -d -p 8000:80 nginx`

### [Set up CI on Redhat and docker](https://github.com/openfrontier/ci)

## Reference:

## [Development Environments with Vagrant and Ansible](https://danielgroves.net/notebook/2014/05/development-environments/)

## [The Zen of High Performance Messaging with NATS](https://www.slideshare.net/wallyqs/the-zen-of-high-performance-messaging-with-nats-strange-loop-2016)

## [Centralized ELK build](https://wiki.zimbra.com/wiki/Centralized_Logs_-_Elasticsearch,_Logstash_and_Kibana)

## [How to serve Django Applications with Apache and mode_wsgi on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-apache-and-mod_wsgi-on-centos-7)

## [Graphite](http://www.aosabook.org/en/graphite.html)

## [httpd Vs. Nginx Vs. HAProxy](http://blog.metrink.com/blog/2014/08/12/httpd-vs-nginx-vs-haproxy/)

## [How to Configure Nginx](https://www.linode.com/docs/websites/nginx/how-to-configure-nginx)

## [Automated Nginx Reverse Proxy for Docker](http://jasonwilder.com/blog/2014/03/25/automated-nginx-reverse-proxy-for-docker/) 

## [Configuring HAproxy as a SwiftStack Load Balancer](https://www.swiftstack.com/wp-content/uploads/2015/06/ConfiguringHAProxyLoadBalancing.pdf)

## [Load balancing MySQL with HaProxy](https://www.percona.com/live/mysql-conference-2013/sites/default/files/slides/haproxy_talk.pdf)

## [Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)

## [confd templates](https://github.com/kelseyhightower/confd/blob/c6622ed25cf9a77804542c6d32c9ee306894f14d/docs/templates.md)

## [OpenSSL Essentials: Working with SSL Certificates, Private Keys and CSRs](https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs)

## [How to Install Zabbix Agent on CentOS/RHEL 7/6/5](http://tecadmin.net/install-zabbix-agent-on-centos-rhel/)
