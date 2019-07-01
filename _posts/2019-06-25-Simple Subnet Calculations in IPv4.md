---
layout: post
title: "Simple Subnet Calculations in IPv4"
date: 2019-06-25
---

Subnetting is the product of logically dividing up a network up into smaller, modular networks. This has many benefits, such as speed increases, easier administration, and increased options for maintaining security. While this post will not go into detail on the benefits or implementation of subnetworks, it is important to be able to calculate subnetwork ranges, network addresses, and broadcast addresses. In other words, "What is the network for for IP ```XXX.XXX.XXX.XXX/XX```? What is the IP range of this network?", and so on.
<hr>  
Before calculating subnets, it is important to know a few terms:  
* IP address: a unique address identifying a device on a network, combining a network address and host address. Each can be identified when the IP is combined with the subnet mask.  
* Network address: the first address in a network, it is the address at which the network resides.  
* Host address: the address of a specific device on a network.  
* Broadcast address: the last address in a network, capable of addressing all devices of the network.  
* Subnet mask: since an IP comprises of a network address and a host address, a subnet mask allows us to see where the split between the two occurs. The subnet mask also defines the range of a subnetwork. 
<hr>
TODO: describe how subnetting works at a binary level, followed by how subnetting works using the "magic number" method. List sources.

Powered by [Jekyll](http://jekyllrb.com)
