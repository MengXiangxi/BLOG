---
title: "Two ways of setting up Hyper-V networks"
categories:
- Coding
tags:
- solution
date: 2023-06-29
layout: post
---

Setting up Hyper-V networks can be tricky, and the current online tutorials do not cover every aspect. Thus, asking ChatGPT results in vein. I realize that the value of human-made tutorial is still non-negligible today, thus I am writing down my intuitive solutions.

## The problem

I am using a Hyper-V virtual machine on a Windows Server 2022. The virtual system is Windows 10 Professional Edition. I want to set up the network so that I would be able to access the internet from the virtual machine.

Although the following solution has only been verified on my OS specified, the same principle applies to other combination of local/VM operating systems.

## My network environment

The hospital uses static IP. As the ethernet socket is limited, we have to use switches as hubs. Currently, we do not require the association of MAC and IP.

Suppose I have a upstream switch whose gateway is `192.168.2.1`, thus, all IPs from this router are assigned in the `192.16.2.x` range with a mask of `255.255.255.0`.

My server has two ethernet interfaces. One of the ethernet interface is connected to the hub, to provide internet access to my host machine. The corresponding IP is `192.168.2.3`. The other LAN port is not in use.

## The solution

There are two types of network architecture. The first one is sharing the local adapter. The second is via bridge.

### Sharing local adapter

Sharing local adapter is the most useful and common way for the VM to connect to the internet. It requires only one internet interface.

![Sharing local adapter]({{ site.baseurl }}/assets/VMNetwork/Sharing.png)

In this method, you first [create a virtual switch](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/connect-to-network), and configure the network sharing of your host LAN connection. They IP of the virtual switch is `192.168.137.1` by default.

Then in the virtual machine, configure the adapter, and the IP should be any address in the IP range of `192.168.137.x`. Here, we suppose the IP is `192.168.137.127`. The gateway should be configured to be `192.168.137.1`.

| Adapter Name | LAN1 | LAN2 | vEthernet Internal | Ethernet |
| --- | --- | --- | --- | --- |
| Computer | Host | Host | Host | Hyper-V |
| Cable ? | Yes | No | N.A. | N.A. |
| IP | 192.168.2.3 | N.A. | 192.168.137.1 | 192.168.137.127 |
| Gateway | 192.168.2.1 | N.A. | Blank | 192.168.137.1 |
| Shared? | vEthernet Internal | N.A. | N.A. | N.A. |

### Via bridge

In this scenario, we have to use a LAN port on the ethernet interface for the virtual machine. Thus, the virtual machine acts like a computer in the same network range as the host. This may avoid the need for network penetration, and allow for direct access of the VM by other machines.

![Via birdge]({{ site.baseurl }}/assets/VMNetwork/Bridge.png)

With this method, if you have $N$ VMs, you need to prepare $N+1$ static, public IP addresses. The cable should also be plugged to the other LAN port on your server, and the corresponding adapter should be set to "bridge" with the virtual adapter. This adapter takes a unique IP, such as `192.168.2.4` here. Then, the remote adapter can be configured just like a local machine.

| Adapter Name | LAN1 | LAN2 | vEthernet Internal | Ethernet |
| --- | --- | --- | --- | --- |
| Computer | Host | Host | Host | Hyper-V |
| Cable ? | Yes | Yes | N.A. | N.A. |
| IP | 192.168.2.3 | N.A. | 192.168.2.4 | 192.168.2.5 |
| Gateway | 192.168.2.1 | N.A. | 192.168.2.1| 192.168.2.1 |
| Shared? | No | N.A. | N.A. | N.A. |

## Conclusion

If you have an extra LAN port, you can have VMs without using DHCP. This can be very convenient, but you are exhausting the IP resources very fast.
