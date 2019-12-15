---
title: ceph
date: 2018-07-06T16:52:44+08:00
lastmod: 2019-08-12T18:01:23+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [How to benchmark rbd performance](https://edenmal.moe/post/2017/Ceph-rbd-bench-Commands/)

```sh
RBD_IMAGE_NAME="bench1"
rbd create --size=10G $RBD_IMAGE_NAME
rbd -p replicapool bench $RBD_IMAGE_NAME --io-type write --io-size 8192 --io-threads 256 --io-total 10G --io-pattern seq/rand
rbd -p replicapool bench $RBD_IMAGE_NAME --io-type read--io-size 8192 --io-threads 256 --io-total 10G --io-pattern seq/rand
```

## Final way to disconnect a POD reference to ceph volume is by restarting the k8s node hosting that POD

## LESSON: The first cephmon node used in ceph storage class `monitors` is critical

When that server is down or too busy, all connections to ceph cluster from k8s POD will suffer. One workaround might be to define an external service for monitors, but at least k8s service solution does not work.


## [ceph hot commands](https://www.cnblogs.com/boshen-hzb/p/6782303.html)

## How to manage local mapped rbd images?

	- to show all mapped rbd: `rbd showmapped`
	- to show one mapped rbd status: `rbd status <pool>/<image> `
	- to remove one rbd mapping: `rbd rm <pool>/<image> `
	- to blacklist a client from accessing os: `ceph osd blacklist add <client inf>`. Please note this command will blacklist all connections from this client, thus will lead to other rbd mapping failure
	- to remove a client from a blacklist: `ceph osd blacklist rm <client info>`
	- to clear blacklist: `ceph osd blacklist clear`

## [rook](https://rook.github.io/docs/rook/v0.9/ceph-quickstart.html) is a good cloud native storage platform that integrates with *ceph*, and is worth for a try

## `ceph pg dump` shows probably the most overwheelming information, and use `ceph pg <pgid> query` to show detail about a specific pg

## `ceph df` to show ceph storage status

## Watch ceph detail message by `ceph -s --watch-debug`

## [Ceph rolling upgrades with ansible](https://ceph.com/geen-categorie/ceph-rolling-upgrades-with-ansible/)

## Install backy2 on centos

```sh
# install gcc, python36, python36-devel and pip3
yum install -y gcc python36 python36-devel python36-pip
pip install backy2

# create a cfg file in the first default path: /etc/backy.cfg, override the following configurations

curl https://raw.githubusercontent.com/wamdam/backy2/af96dfe9d8bae23f96697395c8bd42a6c8aeff91/etc/backy.cfg -O /etc/backy.cfg

```

## TS: `INFO:ceph-create-keys:Cannot get or create admin key, permission denied` when deploying cluster with `ceph-ansible`

This error is due to wrong `monitor_interface`, should set it to the one in `public_network` configuration.

## Backup tool [backy2](https://github.com/labie/backy2)

## [How to resize pv on k8s](https://kubernetes.io/blog/2018/07/12/resizing-persistent-volumes-using-kubernetes/)

## [How to resize a volume](http://docs.ceph.com/docs/mimic/rbd/rados-rbd-cmds/)

The first step is to first mount the target volume locally (make sure to stop other mounts including from k8s POD).
Then use command like `rbd resize --size 3072  kube/kubernetes-dynamic-pvc-17b36f46-3682-11e9-82dc-8ad82d31028c` to resize volume to 3G.
And then grow file system for that volume, like this for xfs: `xfs_growfs /mnt/somepv`, but that make sure there is no active connection to this volume.

## [How to mount a volume to local?](https://blog.programster.org/ceph-deploy-and-mount-a-block-device)

```sh
# map a rbd volume called kubernetes-dynamic-pvc-17b36f46-3682-11e9-82dc-8ad82d31028c in the pool kube
# against monitor node host called ceph2
rbd map kube/kubernetes-dynamic-pvc-17b36f46-3682-11e9-82dc-8ad82d31028c --name client.admin -m ceph2  # this command map remote rbd volume to local device file
mkdir /mnt/somepv
mount /dev/rbd/kube/kubernetes-dynamic-pvc-17b36f46-3682-11e9-82dc-8ad82d31028c  /mnt/somepv
```

To watch rbd status to check who is connecting to the rbd: `rbd status  kube/kubernetes-dynamic-pvc-17b36f46-3682-11e9-82dc-8ad82d31028c`

To unmount:

```sh
umount /mnt/somepv
rbd unmap kube/kubernetes-dynamic-pvc-17b36f46-3682-11e9-82dc-8ad82d31028c
```

## How to list rbd and pools, and show size for a rbd image?

```sh
sudo ceph osd lspools
sudo rbd ls kube  # kube is the pool name
subo rbd info kube/<image-id>  # kube is the pool, followed by image id
```
