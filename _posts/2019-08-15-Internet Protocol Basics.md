---
layout: post
title: "Internet Protocol Basics"
date: 2019-08-15
---

IP, or Internet Protocol, is the network layer protocol which handles internetwork delivery over TCP/IP networks, such as the internet. This process is connectionless and is referred to as internetwork datagram delivery, using IP addresses much like a postal system would route mail using physical addresses. Since it is connectionless, it is important to remember that IP does not return information about what has been sent, what has been received, or what has not been received. IP simply takes TCP or UDP data, encapsulates it with the IP header, and ships it out to the specified location. Remembering the TCP/IP model, this looks a bit like so:  
```  

Layer 5 [Data]  # Basic application data  
Layer 4 [TCP/UDP header] + [Data]  # Application data is packaged with tcp/udp header  
Layer 3 [IP header] + [TCP/UDP header + data]  # Result is packaged with IP header  
Layer 2 [Frame header] + [IP, TCP/UDP header + data]  # Result is packaged with frame header

```  

<hr>
<h2>Addressing</h2>  
IP addresses are assigned at a network interface level. This is an important distinction as devices can technically have multiple IPs: one per network interface. For instance, say you have both a wired and wireless connection on a machine, such as a Wifi interface and an Ethernet interface. Each will have its own IP address. On a given internetwork, each IP address must be different. An IP address is configured and assigned in a handful of ways, either static or dynamic. With static IP addressing, an IP address will not change, but with dynamic addressing (such as DHCP) IP addresses may be changed by software without manual interference.  

Addresses themselves are just an identifying binary number. Currently, these are usually 4 octet 32-bit numbers (IPv4), allowing for an addressing space of 2^32 different addresses. These are more commonly represented in decimal value. IPv6 is a 16 octet 128-bit number, allowing for a substantially larger addressing space of 2^128 addresses, and is commonly represented in hex value. Many of the addresses in these spaces are reserved however, and as address space began to dwindle for IPv4, IPv6 was developed.  

<hr>  
<h2>Routing</h2>  
An IP address itself is comprised of a network prefix and a host address. The network prefix identifies what network the host can be found on, and the host address is the specific network interface. This can be thought of similarly to a home address: If you live on 123 Anywhere Street, Anywhere Street would be the network address, and 123 would be the host address. Many homes may belong to the Anywhere Street network, however for mail to be routed only one home can be assigned 123. At a basic level, when a packet is being routed, there are several steps taken:  

+ The sending network interface checks to see if a packet is bound for the same network. If not, it sends the packet to it's router.  
+ The router de-encapsulates the Ethernet frame to look at the IP header info. If the header does not match a connected network, the router encapsulates the packet into an Ethernet frame and uses it's routing table to route the packet to a network address (another router) which would include the network. This step may repeat several times, over several routers.  
+ The router de-encapsulates the Ethernet frame to look at the IP header info. Since it matches a connected network, the router encapsulates the packet once more and forwards it to the receiving network interface, and passes the IP level encapsulated packet to the OS.  

Powered by [Jekyll](http://jekyllrb.com)  
