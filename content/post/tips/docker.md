---
title: "docker"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-12-06T17:10:09+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## How to allow nobody user running inside container to acccess outside volume?

VOLUME /etc/alerts.d

## A good explaination on [why we should use gosu/su-exec in docker](https://medium.com/better-programming/running-a-container-with-a-non-root-user-e35830d1f42a)

## In docker-compose file, `ports` will bind to localhost IPV6 IP address by default

That's why you will get `connnection reset` error when you try to `curl` to `127.0.0.1:<some port>`, the resolution is to explictly bind to `127.0.0.1` in `ports` like this: `- 127.0.0.1:8088:8088`. During trouble shooting process, it's critical to capture network packages with *wireshark*

## Something about [ENTRYPOINT](https://oprea.rocks/blog/how-to-properly-override-the-entrypoint-using-docker-run/)
> The ENTRYPOINT gives a container its default nature or behavior, so that when you set an ENTRYPOINT you can run the container as if it were that binary, complete with default options, and you can pass in more options via the COMMAND.
> But, sometimes an operator may want to run something else inside the container, so you can override the default ENTRYPOINT at runtime by using a string to specify the new ENTRYPOINT
> If the image also specifies an ENTRYPOINT then the CMD or COMMAND get appended as arguments to the ENTRYPOINT.

Basically, if want to add more options, that should be passed as CMD

## Looks like for Java app, it is better to create a `VOLUME /tmp` to [speed up second launch](https://www.surevine.com/building-docker-images-with-maven/)

> We create a /tmp volume to speed up second launch times of the containers, as this is where the embedded application container stores its exploded contents to.

## Clean up unused docker images and exited containers

```sh
# clean up exited containers
docker system prune -af --volumes
# clean up unused docker images
# looks like the previous command also reclaims unused images
docker image prune -af

```

## [Understand container communiction](https://docs.docker.com/v17.09/engine/userguide/networking/default_network/container-communication/)
> IP packet forwarding is governed by the ip_forward system parameter

Docker engine options that matter for networking:
	- `--iptables`: Docker will never make changes to your system iptables rules if you set --iptables=false when the daemon starts.
	- `--icc`: If the Docker daemon is running with both --icc=false and --iptables=true then, when it sees docker run invoked with the --link= option, the Docker server will insert a pair of iptables ACCEPT rules so that the new container can connect to the ports exposed by the other container – the ports that it mentioned in the EXPOSE lines of its Dockerfile

*Further reading*: need deep understanding about `iptable` rules and `netfilter`, probably [here](https://www.linuxtopia.org/Linux_Firewall_iptables/a6831.html#LARTC)
*Must read*: [Netfilter Architecture](https://www.netfilter.org/documentation/HOWTO/netfilter-hacking-HOWTO-3.html) and book *Linux.iptable.Pocket.Reference*

## Use `--add-host` build option to provide host to IP mapping when executing [`docker build`](https://docs.docker.com/engine/reference/commandline/build/)

## It is possible to use `build-arg` to provide `HTTP_PROXY`, `HTTPS_PROXY` which is called engine buildin variables to docker build engine as mentioned in [issue 14634](https://github.com/moby/moby/issues/14634)

```sh
# example usage
docker build --build-arg 'HTTP_PROXY=proxyserver:1087' --build-arg 'HTTPS_PROXY=proxyserver:1087' -t hyperledger/fabric-peer .build/image/peer
```

## docker now provides [secrets](https://docs.docker.com/engine/swarm/secrets/) concept as well as config like k8s does.

The secret will be mounted as in memory filesystem (which means that it won't be committed to container), and mount to `/run/secrets/<secret_name>` by default, that's how jenkins master docker does for initial password

The article also demonstrated  solid steps to generate self signed root ca certificate and site certificate.

## How to upgrade docker engine?

For docker-ce, before upgrade, please make sure to backup `/etc/docker/daemon.json`,which contains docker's storage options like this:

```js
{
  "ipv6": false,
  "storage-driver": "overlay2",
  "graph": "/mnt/docker"
}
```

But a better option is to add this to `/etc/systemd/system/docker.service.d/docker-options.conf` like this, and looks like those files won't be removed when upgrading:

```sh
[Service]
Environment="DOCKER_OPTS=  --graph=/data/docker --log-opt max-size=50m --log-opt max-file=5 --iptables=false"
```

## Show container storage usage: `docker ps -as`

## How to get docker image hash?

```sh
docker inspect --format='{{index .RepoDigests 0}}' $IMAGE
```
## How to change time zone for container?

Just add *TZ* environment variable like this in POD:

```yaml
	- name: TZ
	  value: Asia/Shanghai
```

## How to add current user to docker group?
```
# if docker group does not exist
sudo groupadd docker
# add user jenkins to docker group
sudo usermod -aG docker jenkins
sudo chown root:docker 
```
## Trouble shooting "dial tcp: lookup dseasb33srnrn.cloudfront.net on 192.168.65.1:53: no such host"
Hack is to chanage DNS server by editing */etc/resovl.conf* by adding *8.8.8.8* to nameserver, and might also need to remove other nameserver.


## Trouble shooting oci API error
```
2017-07-04 05:20:16.065 UTC [msp/identity] Sign -> DEBU 009 Sign: digest: 3FDB1E9E82C048A02242D3701D9298A1FAD922F4C918E51E47D9606484242AD9
Error: Error endorsing chaincode: rpc error: code = 2 desc = Error starting container: API error (500): {"message":"oci runtime error: container_linux.go:262: starting container process caused \"process_linux.go:339: container init caused \\\"read init-p: connection reset by peer\\\"\"\n"}
```
**FIX**: it is caused by *docker-ce*

## How to delay start up until another containter is working?
There is no native way to achieve this. One [workaround](http://stackoverflow.com/questions/31746182/docker-compose-wait-for-container-x-before-starting-y) is:
```
Dockerfile

FROM python:2-onbuild
RUN ["pip", "install", "pika"]
ADD start.sh /start.sh
CMD ["/start.sh"]
start.sh

#!/bin/bash
while ! nc -z rabbitmq 5672; do sleep 3; done
python rabbit.py
```
## Native way to get some docker information by unix socket
`echo -e "GET /containers/json?all=0 HTTP/1.0\r\n" | nc -U /var/run/docker.sock`
## [Comparing Seven Monitoring Options for Docker](http://rancher.com/comparing-monitoring-options-for-docker-deployments/)
## [Deploy a docker registry 2.0](https://github.com/docker/docker.github.io/blob/master/registry/deploying.md)
## [How to create a docke base ppc64le centos image?](http://cloudgeekz.com/752/centos-docker-image-power.html)
```
mkdir /centos7-root
export rpm_root=/centos7-root
export rpm_repo=http://mirror.centos.org/altarch/7/os/ppc64le
rpm --root ${rpm_root} --initdb
rpm --root ${rpm_root} -ivh ${rpm_repo}/Packages/centos-release-7-2.1511.el7.centos.2.9.altarch.ppc64le.rpm
rpm --root ${rpm_root} --import ${rpm_repo}/RPM-GPG-KEY-CentOS-SIG-AltArch-7-ppc64le
rpm --root ${rpm_root} --import ${rpm_repo}/RPM-GPG-KEY-CentOS-7
yum -y --installroot=${rpm_root} install yum
# change root and do other customization as you want
chroot ${rpm_root} /bin/bash
# save the image
tar -C ${rpm_root}/ -cJvf centos-7.2.1511-docker.tar.xz .
# later, import it
cat centos-7.2.1511-docker.tar.xz | docker import - centos7le
```
## [How to deploy a docker registry on CentOS](https://www.server-world.info/en/note?os=CentOS_7&p=docker&f=6)
```
yum -y install docker-registry
vi /etc/docker-registry.yml
# line 19: add
search_backend: _env:SEARCH_BACKEND:sqlalchemy
# line 21: specify DB file for search (change it if need)
sqlalchemy_index_database: _env:SQLALCHEMY_INDEX_DATABASE:sqlite:////tmp/docker-registry.db
# line 74: the directory to store images (change it if need)
storage_path: _env:STORAGE_PATH:/var/lib/docker-registry
# create a directory to store images
mkdir /var/lib/docker-registry
systemctl start docker-registry
systemctl enable docker-registry
# make sure to be able to access
```
## [How to add certificates for docker registry](https://www.server-world.info/en/note?os=CentOS_7&p=docker&f=7)
```
cd /etc/pki/tls/certs
make server.key
make server.csr
openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650
```
## [Setup a Docker Private Registry on POWER Servers running Linux](https://www.ibm.com/developerworks/community/blogs/fe313521-2e95-46f2-817d-44a4f27eba32/entry/Build_and_Setup_a_Private_Docker_Registry_on_Power_Servers?lang=en)
## How to get catalog information from private registry?
    + List all iamges:`curl http://<registry_host>:5000/v2/_catalog`
    + Show tags for some image:`curl http://<registry_host>:5000/v2/<image_name>/tags/list`
## [How to install latest docker engine on centos?](https://docs.docker.com/engine/installation/linux/centos/)
Install latest: `curl -fsSL https://get.docker.com/ | sh`
Enable auto-start and start it: `systemctl enable docker; systemctl strart docker`
## How to load a raw/tar file?
Like this: `docker load -i  /home/opuser/centos_7.2_docker_aufs_ppc64le_zb_v4.raw`
## Use socks5 as proxy for pulling image?
Run docker compose like this:
`HTTP_PROXY=socks5://localhost:10086 docker-compose up`
## Preferred remote file copy for docker file
Instead of:
```
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
Write docker file like the following:
```
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```

## You can specify _--entrypiont_ to override default entrypoint defined in docker file
Like this: `docker run -it --name portal -p 80:8080 --entrypoint="/bin/bash" cds/portal`

## [Docker registry and registry mirrow](http://www.oschina.net/news/57894/daocloud)
Use docker registry mirror:
```
echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://0acd8e64.m.daocloud.io\"" | sudo tee -a /etc/default/docker
sudo service docker restart
```

## How to mount volumes from an existing container?
Assume a busybox (volume container) is already running with volume /data:
`docker run --name bb -v /data busybox`
For the new container, run with --volumes-from option like the following:
`docker run --volumes-from bb --rm cnetos7.2 touch /data/me.log`
The above command will create a new file called me.log under /data

## How to detach from docker tty (interactive mode)?
Press the escape sequene: 
```
Ctrl+p, Ctrl+q
```
## How to attach to a running container?
Use `docker attach <name of docker>`

## How to mount host volume to container?
Still leverage -v option like the following:
`docker run --name bb2 -v /data/:/data ppc64le/busybox`

Interactive mode:
`docker run -it --name bb2 -v /data/:/data ppc64le/busybox sh`

## Installation on redhat/centos
```
sudo tee /etc/yum.repos.d/docker.repo <<-EOF
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
sudo yum install docker-engine
```

## References:
## [Why Docker is Not Yet Succeeding Widely in Production](http://sirupsen.com/production-docker): deep dive into various aspects of docker
## [Docker build reference](https://docs.docker.com/engine/reference/builder/)
## [Best practices for writing Dockerfiles](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)
## [How To Install and Use Docker Compose on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-14-04)
## [Compose file reference](https://docs.docker.com/compose/compose-file/)
## [Docker Clustering Tools Compared: Kubernetes vs Docker Swarm](https://technologyconversations.com/2015/11/04/docker-clustering-tools-compared-kubernetes-vs-docker-swarm/)
## [Flocker on CentOS 7](https://media-glass.es/2015/12/13/flocker-centos-7/)
## [DOC madness](http://container42.com/2014/11/18/data-only-container-madness/)
## [Persistent volumes with Docker - Data-only container pattern](http://container42.com/2013/12/16/persistent-volumes-with-docker-container-as-volume-pattern/)
## [Walkthrough: Docker Volumes vs Docker Volumes with Flocker](https://clusterhq.com/2015/12/09/difference-docker-volumes-flocker-volumes/)
## [Docker Compose with example for django and postgres](https://developer.fedoraproject.org/tools/docker/compose.html)
