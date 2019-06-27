---
layout: post
title: "Notes on the OSI Model"
date: 2019-06-23
---

The Open Systems Interconnection (OSI) model is a reference model used to describe various related concepts in computer networking. The OSI model provides a framework of layers which describe communications functions that may be performed by any number of protocols, though it should be noted that not every layer is always needed. Each layer represents its own abstraction which interfaces in some form with the layers adjacent to itself. A layer serves the layer above it and is served by the layer below it. For example, the layer that ensures error-free communications across a network provides the path needed by applications above it, while it calls the next lower layer to send and receive packets that constitute the contents of that path. Multiple instances of layers are possible (such as multiple application layers operating simultaneously, but not dependent on one another). Running discord whilst using filezilla would create multiple abstractions of the application layer which are not hierarchically dependent upon one another.  

| Layer | Name | Description |Technologies|Data Unit|Layer Type|  
|-------|-------|-------|-------|-------|-------|  
| 7 | Application | Network process to computer programs| FTP, HTTP, SMTP | Data | Host |  
| 6 | Presentation | Data representation, security encryption, convert computer code to network formatted code | AFP, MIME | Data| Host  |  
| 5 | Session | Interhost communication, managing sessions between programs | X255, SCP | Data | Host |  
| 4 | Transport | End-to-end connections, reliability and flow control| TCP | Segment | Host |  
| 3 | Network | Path determination and logical addressing | IP | Packet | Media |  
| 2 | Data Link | Physical addressing | ARP, PPP | Frame | Media |  
| 1 | Physical | The physical infrastructure used to send and receive signals | USB, Bluetooth  | Bit | Media |  

<h2>Layer 7: Application layer</h2>
The application layer provides an interface for the end user operating a device connected to a network. It is not in itself an application but represents the component within the application which controls communication with other devices. This masks the rest of the application from the data transmission, relying upon all the layers below to complete this process. The data is at this stage presented in a way the user can understand.  
* Remote login to hosts: Telnet  
* File transfer: File Transfer Protocol (FTP), Trivial File Transfer Protocol (TFTP)  
* Electronic mail transport: Simple Mail Transfer Protocol (SMTP)  
* Networking support: Domain Name System (DNS)  
* Host initialization: BOOTP  
* Remote host management: Simple Network Management Protocol (SNMP), Common Management Information Protocol over TCP (CMOT)  
___    
<h2>Layer 6: Presentation layer</h2>
The presentation layer takes the information that the application layer of one system sends out and ensures that it is readable by the application layer of another system. This is often needed for services such as translation between different types of machines, data compression, and encryption (though not all encrption is handled at the presentation layer). If necessary, the presentation layer might be able to translate between multiple data formats by using a common format.  
* Multipurpose internet mail extensions (MIME)    
* External data representation (XDR)  
* Secure Sockets Layer (SSL)  
---  
<h2>Layer 5: Session layer</h2>
This layer establishes request/response communication. When needed a session is started with authentication, after which a request is sent. After a response the session might be ended or a new request is sent. This is the first layer where a client/server concept is introduced. A specific device might change the role from client to server or vice versa. Essentially, the session layer responds to service requests from the presentation layer and issues service requests to the transport layer, often communicated through application program interfaces (APIs). 
* SOCKS  
* Session announcement protocol (SAP) 
* Network basic input/output system (NETBIOS)  
---  
<h2>Layer 4: Transport layer</h2>
The Transport Layer is the level at which system reliability and quality are ensured. This layer manages traffic flow through the network layer to reduce congestion on a network. Since the lower layers handle the actual delivery of data between devices, the transport layer performs error checking, ensuring quality of service by resending data when data has been corrupted (acknowledgements and retransmission timers are a common method). Some methods of encryption and firewall security take place on this layer. The protocols of this layer provide host-to-host communication services for applications. Since many applications may be running and sending or receiving data simultaneously, the transport layer must be able to combine the data in a manner which can later be reversed by the receiving device so that the data is recieved by the correct processes (multiplexing and demultiplexing).  
* Transmission control protocol (TCP)  
* User datagram protocol (UDP)  
---  
<h2>Layer 3: Network layer</h2>
The Network layer provides the means of transferring variable-length network packets from a source to a destination host via one or more networks. Within the service layering semantics of the OSI network architecture, the network layer responds to service requests from the transport layer and issues service requests to the data link layer. The Network (or routing) Layer works to coordinate related parts of a data conversation to ensure that large files are transferred. In other words, while the data link layer deals with the method in which the physical layer is used to transfer data, the network layer deals with organizing that data for transfer and reassembly via datagram encapsulation. This layer also handles aspects of Routing Protocols, finding the available [best] path(s) from one network to another to ensure delivery of the data.  
* Internet protocol (IPv4, IPv6)    
---  
<h2>Layer 2: Data link layer</h2>
This layer is the protocol layer that transfers data between adjacent network nodes in a wide area network (WAN) or between nodes on the same local area network (LAN) segment. The Data link layer is mainly the method in which information from the network is broken down into frames and transmitted over the physical layer. This layer is also responsible for some error detection and correction; and some addressing so different devices can tell each other apart in larger systems. Examples of data link protocols are Ethernet for local area networks (multi-node), the Point-to-Point Protocol (PPP), HDLC and ADCCP for point-to-point (dual-node) connections. In the Internet Protocol Suite (TCP/IP), the data link layer functionality is contained within the link layer, the lowest layer of the descriptive model, which also includes the functionality encompassed in the OSI model's physical layer.  
* Ethernet protocol (IEEE 802.3)  
* Point-to-point (PPP)  
* Media Access Control (MAC)  
---  
<h2>Layer 1: Physical layer</h2>
The physical layer translates logical communications requests from the data link layer into hardware-specific operations to cause transmission or reception of electronic signals. In other words, this is the layer where data is actually moved across the network. It specifies how a device sends and receives information, such as using copper wires or fiber-optic cables. Examples of this include Ethernet or fiber optic cables, phone cords used for dial-up or DSL services, the coaxial cable used to provide broadband internet, the wires used to connect various components of a computer or even the radio signals used in wireless communication. Other functions of the physical layer include the conversion of signals into something that another layer can use (referred to as a bit) and adjusting the signal to allow for multiple users to use the same connections. Within the semantics of the OSI model, the physical layer translates logical communications requests from the data link layer into hardware-specific operations to cause transmission or reception of electronic signals.  
* Ethernet protocol (IEEE 802.3)  
* Universal serial bus (USB)  
* Bluetooth  
---  
<h3>Sources and further reading:</h3>
[Wikipedia - OSI Model](https://en.wikipedia.org/wiki/OSI_model#Protocol_layers_(Brief))  
[CWNP - Why Does the OSI Reference Model Matter?](https://www.cwnp.com/cwnp-wifi-blog/whytheosimodelmatters/)  
[Cisco - OSI Model Reference Chart](https://learningnetwork.cisco.com/docs/DOC-30382)  
[The TCP/IP Guid - OSI Reference Model](http://www.tcpipguide.com/free/t_OSIReferenceModelLayers.htm)  

Powered by [Jekyll](http://jekyllrb.com)
