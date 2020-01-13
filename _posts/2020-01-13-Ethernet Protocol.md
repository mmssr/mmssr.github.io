---
layout: post
title: "Ethernet Protocol"
date: 2020-01-13
---

Ethernet is used to connect machines together, and is the most common way of establishing a LAN. The ethernet protocol suite overlaps with multiple network layers on the OSI model, both the data link layer (2) and the physical layer (1). As an example, at the lowest layer (physical,) we have the various streams of bits traversing an ethernet cable. These bits themselves do not identify devices, such as the destination or source of the bits. That functionality happens at the data link layer.  
<hr>  
<h2>Ethernet Frames</h2>
Let's recall that routing amidst networks occurs at layer 3, switching within a network occurs at layer 2, and the cable exists at layer 1. At the data link layer (2) we interpret those streams of bits from layer 1 as ethernet frames. Ethernet frames within the IEEE 802.3 standard comprize of a few different fields, and are of varying sizes. The first part is the preamble, 7 bytes which indicate a frame is beginning so that everything can be synced up.  Then, the Start Frame Delimiter (1 byte) indicates that the destination MAC address is next. The destination and source MAC addresses follow, each are 6 bytes long. These MAC addresses exist to give the network access controller a specific destination so that the switch will be able to route traffic, and they need to be unique for the protocol suite to properly function. It might seem sort of redundant to an IP, but keep in mind that an IP = internet layer = router and is intended for routing between multiple networks, while a MAC address is at layer 2 = network layer = switches and is intended to handle traffic within the network.

After that, the type of protocol within the frame is specified so (2 bytes) so that the data can be properly parsed and interpreted. The payload data within would be your application, transport, and internet layer data encapsulated together. At the very end is a 4 byte Frame Check Sequence. The Frame Check Sequence is calculated before being sent, and is based upon the rest of the frame. Since it is a calculation based upon that data, when the frame arrives at its destination it can be recalculated by the recipient. If the FCS matches, then the packet can safely be assumed to have been sent correctly. If the FCS does not match, the frame can be assumed to have been corrupted in some fashion and frame can be dropped. 

```

[Preamble - 7 bytes][Start Frame Delim - 1 byte][Dest Mac - 6 bytes][Source Mac- 6 bytes][Type - 2 bytes][Payload- var][Frame Check Seq - 4 bytes]

```  

Powered by [Jekyll](http://jekyllrb.com)  
