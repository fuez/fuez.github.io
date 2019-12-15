---
title: cloud
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-05-08T17:39:21+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## When install rsa key to qcloud, make sure to logon with user `ubuntu` if target server is ubuntu linux

 for Heroku
-[Getting started with Heroku](https://dashboard.heroku.com/apps)
-[Getting started with python](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)

### Try out
## Create a sample project or just clone _https://github.com/heroku/python-getting-started.git_
## Enter projectroot
    + Create a sample project by: `heroku apps:create hello-andy`, and output is like:
```
Creating ⬢ hello-andy... done, stack is cedar-14
https://hello-andy.herokuapp.com/ | https://git.heroku.com/hello-andy.git
```
## Additional remote will be added automatically like the following:
```sh
heroku  https://git.heroku.com/hello-andy.git (fetch)
heroku  https://git.heroku.com/hello-andy.git (push)
origin  https://github.com/heroku/python-getting-started.git (fetch)
origin  https://github.com/heroku/python-getting-started.git (push)
```
## Push to remote by: `git push heroku master`
## Ensure at least one app instance is running: `heroku ps:scale web=1` 
## Open app by: ` heroku open`
## And you can check logs by: ` heroku logs --tail`

## openstack tips
## [Neutron support for VRRP protocol](https://bugs.launchpad.net/neutron/+bug/1475717)

## [How to reboot an errored instance?](http://docs.openstack.org/user-guide/cli_reboot_an_instance.html)
```sh
nova rescue --image <imageName> <SERVER>
# or nova rescue --rescue_image_ref IMAGE_ID SERVER
nova unrescue SERVER
```
## How to check default secgroup?
`nova secgroup-list-rules default`

## How to allow certain ports?
`nova secgroup-add-rule default tcp 5044 5044 0.0.0.0/0`

## How to update ports to allow binding to virtual IP?
```sh
neutron port-list | awk -F '|' '$5 ~/192.168.32.23/ {gsub(/ /, "", $4);cmd="neutron port-update  "$2" --name hds_dev1 --allowed-address-pairs type=dict list=true mac_address="$4",ip_address=192.168.32.255"; print(cmd)}'
```
Refer to [Bind to multiple virtual ip addresses](https://ask.openstack.org/en/question/84676/how-to-add-multiple-ip-inside-allowed-address-pairs/)

Make sure to check whether port is updated by command like:
`neutron port-show 4367b456-7c87-4c5e-acb0-20708a702ee7` and  check _ allowed_address_pairs_ line.


## How to create a  virtual IP?

* create fixed ip (internal IP)
```
 neutron port-create 9d8b2e30-1bd2-4bda-b44b-33c09bb45b87`
==> 
| fixed_ips             | {"subnet_id": "120e1ed4-0eb3-4b04-b734-454493bc416e", "ip_address": "10.9.37.72"} |
| id                    | d97fd7d1-84ff-4c54-b9cb-d35dba27b973
```
* create floating ip
```
nova  floating-ip-create ext_net2
==> 172.16.14.205
```
*  get floating ip address
```
nova --debug floating-ip-list
# Find id from trace:
{"instance_id": null, "ip": "172.16.14.205", "fixed_ip": null, "id": "f5389d43-ef70-42a1-895a-3a279293f92b", "pool": "ext_net2"}
```

*  associate floating IP with fixed ip
```
neutron floatingip-associate f5389d43-ef70-42a1-895a-3a279293f92b  d97fd7d1-84ff-4c54-b9cb-d35dba27b973
```

* binding to KVM
```
neutron port-list | awk -F '|' '$5 ~/10.9.35/ {gsub(/ /, "", $4);cmd="neutron port-update  "$2" --name hds_comm --allowed-address-pairs type=dict list=true mac_address="$4",ip_address=10.9.37.72"; print(cmd)}'
```

## How to rebuild a KVM with volume attached?
First, find attached volume's id and KVM's id by querying nova and cinder
```
nova list 
cinder list
```
with ids, run `nova volume-detach` like the following:
```
nova --debug volume-detach 0d26879c-e6ee-42f7-8de3-981767d12a3a  31ec0935-eae6-4186-bf9c-4a6d1eee4c68
```
## [How to dynamically scale KVM CPU/Memory?](https://www.semanticlab.net/index.php/KVM_increase_CPU_and_RAM_during_runtime)

## References:
## [Implementing High Availability Instances with Neutron using VRRP] (http://superuser.openstack.org/articles/implementing-high-availability-instances-with-neutron-using-vrrp)
