---
title: "networking"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-05-21T14:07:18+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [Understand CNAME record](https://en.wikipedia.org/wiki/CNAME_record)

## [What is EVPN](https://www.securelink.be/evpn-next-generation-datacenter-interconnect/)

EVPN stands for Ethernet Virtual Private Network.
	- With EVPN, it is possible to stretch L2 connections across multiple locations without adverse consequence
	- An L3 link is realized between datacenters. Within this L3 link, Multiprotocol Border Gateway Protocol Ethernet Virtual Private Network (MP-BGP EVPN) is used. This protocol is used as an underlay & control plane and it is responsible for the distribution of the learned MAC and IP-addresses between the various components. VXLAN is used as an overlay and data plane.

## [What is VxLAN](https://www.securelink.be/vxlan-overlay-protocol/)

VXLAN stands for Virtual Extensible LAN. VXLAN is an encapsulation technique in which layer 2 ethernet frames are encapsulated in UDP packets. VXLAN is a network virtualization technology. 
	- The different VXLAN networks are provided with a VXLAN Network Identifier. This VNI number is similar to a VLAN ID.
	- The VNI identifies the layer 2 Ethernet frame segment. Virtual machines that are linked to each other by the VNI are able to communicate with each other on layer 2.
	- VXLAN is a technique that is used in an overlay network.The VXLAN tunnels are set up on top of the underlay network. Modern data centers are also often equipped with a reliable and scalable L3 clos architecture on which the VXLAN tunnels can be transported.
	- It is of great importance that the MTU of the underlay is adjusted to 1600 instead of the standard MTU. Without this adjustment VXLAN cannot functioin
	- With VXLAN, layer 2 networks can be stretched across L3 underlay networks

## [What is CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)

It is called classless inter-domain routing.
And the CIDR number comes from the number of 1's in the subnet mask when converted to binary.

## [OSI layered model](https://smallbiztrends.com/2013/09/osi-model-layer-networking.html)
The OSI stands for Open System Interconnection, which is a networking model comprised of seven "layers".

Layer 2 Data Link: Responsible for physical addressing, error correction, and preparing the information for the media
Layer 3 Network: Responsible for logical addressing and routing IP, ICMP, ARP, RIP, IGRP, and routers

## Basic operation on iptables
    + List all rules: `iptables -L` or for certain chain:`iptables -L IN_public_allow` ; use __-n__ option to display ip address and port number in numeric format; use __-v__ to display detail information; use __--line-numbers__ option to display rule number.
    + Add/append one rules: `iptables -A <chain> ...`
    + Insert one rules: `iptables -I <chain> <position> ...`
    + Delete one rule: `iptables -D <Chain> <rule number>`
    + Save current rules: `iptables-save`, which is also a good way to list rules
    + Restore from saved one: `iptables-restore < [saved.rules]`
    + Block an IP address: `iptables -I INPUT 1 -s 1.2.3.4 -j DROP`; make sure to insert one rule at position 1
    + Only allow some IP address to access some port: `iptables -I INPUT -p tcp -s ! yourIPaddress --dport 22 -j DROP`
## How to allow public access to certain ports?
```
iptables -A IN_public_allow -m ctstate --state NEW -m tcp -p tcp --dport 7000:7010 -j ACCEPT
```

## Reference:
## [Linux basic introduction about ip tables](http://www.thegeekstuff.com/2011/02/iptables-add-rule/)
## [Good iptables examples](http://www.cyberciti.biz/tips/linux-iptables-examples.html)
## [How to check public IP for ADSL](https://www.chiphell.com/thread-821248-1-1.html)
## [Quick-Tip: Linux NAT in Four Steps using iptables](http://www.revsys.com/writings/quicktips/nat.html)
