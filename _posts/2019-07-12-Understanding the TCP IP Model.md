---
layout: post
title: "Understanding the TCP/IP Model"
date: 2019-07-12
---

The TCP/IP suite is a collection of protocols which define how most networking is accomplished, with the most common example being the internet. Much like OSI, TCP/IP has its own reference model with which we can describe the layers of communication functions which comprise the TCP/IP suite. The model works in much the same way, with each layer representing its own abstraction which interfaces in some form with the layers adjacent to itself; a layer serves the layer above it and is served by the layer below it. One difference, however, is that TCP/IP is frequently described as both a 4 layer model and as a 5 layer model. This explanation will explore the latter.  

| Layer | Name | Description | Technologies | Data Unit | OSI Equivilent |  
|-------|-------|-------|-------|-------|-------|  
| 5 | Application | Provides application services to users and programs | HTTP, SIP, IMAP | Data | 5, 6, 7 |  
| 4 | Host-to-Host Transport | Provides the data stream carried between the network to and the application | TCP, UDP | Segment | 4  |  
| 3 | Internet | Provides the network addressing and routing over a network | IPv4, IPv6, ICPM | Packet | 3 |  
| 2 | Network Interface | Provides whatever network inferface the IP will work over | MAC, IEEE 802.3, PPP  | Frame | 2 |  
| 1 | Physical* | Hardware which provides for physical transmission | USB, Bluetooth  | Bit | 1 |  

*In the 4 layer model, the physical layer is assumed to be a part of the network interface layer. 

TODO: describe each layer, and the journey of a request. 

Powered by [Jekyll](http://jekyllrb.com)
