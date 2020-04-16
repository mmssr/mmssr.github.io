---
layout: post
title: "Understanding the TCP/IP Suite Implementation"
date: 2019-07-12
---

The TCP/IP suite is a collection of protocols which define how most networking is accomplished, with the most common example being the internet. Much like OSI, TCP/IP has its own model with which we can describe the layers of communication functions comprising the TCP/IP suite. The model works in much the same way, with each layer representing its own abstraction which interfaces in some form with the layers adjacent to itself. Outgoing data is progressively encapsulated with additional headers at each level of the layer so that the data can be delivered, and incoming data is progressively decapsulated to deliver the data to the destination network application. One difference is that TCP/IP is frequently described as either a 4 layer model or as a 5 layer model. This explanation will explore the latter.  

| Layer | Name | Description | Technologies | Data Unit | OSI Equivilent |  
|-------|-------|-------|-------|-------|-------|  
| 5 | Application | Provides application services to users and programs | HTTP, DNS, NFS, FTP, IRC, etc | Data | 5, 6, 7 |  
| 4 | Transport | Provides the data stream carried between the network to and from the application | TCP, UDP | Segment | 4  |  
| 3 | Internet | Provides the network addressing and routing over a network | IPv4, IPv6, ICMP, ND, etc | Packet | 3 |  
| 2 | Network | Provides whatever network inferface the IP will work over | SLIP, PPP, MAC | Frame | 2 |  
| 1 | Physical* | Hardware which provides for physical transmission | Hardware Drivers  | Bit | 1 |  

*In the 4 layer model, the physical layer is assumed to be a part of the network interface layer.  
<hr>  
<h2>Application Layer</h2>  
When a message of some sort is sent with TCP/IP, the message begins at the application layer, with something such as an HTTP or DNS request. The message itself will be formatted in a manner in which it can be sent with either TCP or UDP.  
<hr>  
<h2>Transport Layer</h2>  
The transport layer receives this data from the application layer, and will specify a port (memory location) for the sending and receiving of data as well as formatting the data either into a TCP segment or a UDP datagram. In TCP/IP, the two main transport layer protocols are TCP and UDP. In simple terms, TCP establishes a connection whereas UDP is connectionless. With TCP, error checking will occur so that packets cannot be dropped or received out of order. This requires a connection, as error handling requires responses from the recipient. With UDP there is no guarantee of message delivery, and it therefore does not require a connection. If a TCP segment is specified, the TCP SYN-ACK three way handshake will occur, and any errors will be handled by the protocol itself. On the other hand, if a UDP datagram is specified, the data will simply be sent, and the application layer will handle an error if needed (for example, a dropped DNS request will simply result in an error inside your web browser.)  
<hr>  
<h2>Internet Layer</h2>  
Once the transport layer has created either a TCP segment or a UDP datagram, the internet protocol will format the segment or datagram into an IP datagram by attaching its own header. This includes the sequence order, the IP address of both machines, and the length of the datagram.  
<hr>  
<h2>Network (or Data Link) Layer</h2>  
The network layer takes the IP datagram sent from the internet layer and formats the datagram into frames, adding hardware addressing. Following this, the physical layer handles the actual transmission of the bits.
<hr>  
<h2>Physical Layer</h2>  
Technically, the scope of the TCP/IP protocols does not extend to the physical layer, but once the other protocols have progressively added their headers to the message being sent, the physical layer will handle the physical transmission. Keep in mind, many descriptions of the TCP/IP model refer to the physical layer as being a part of the network layer.  

Powered by [Jekyll](http://jekyllrb.com)
