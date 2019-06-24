---
layout: post
title: "Notes on the OSI Model"
date: 2019-06-23
---

The Open Systems Interconnection (OSI) model is a method used to describe computer networking. The OSI model creates a common description for how networks function, which is necessary for mutual understanding of function and description. Each layer represents it's own abstraction which interfaces in some form with the layers adjascent to itself. A layer serves the layer above it and is served by the layer below it. For example, a layer that provides error-free communications across a network provides the path needed by applications above it, while it calls the next lower layer to send and receive packets that constitute the contents of that path. Keep in mind, multiple instances of layers are possible (such as multiple application layers operating simultaneously, but not dependent on one another. Running discord whilst using filezilla, for instance, would be multiple abstractions which are not heirarchially dependent upon one another.)  

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
The application layer provides an interface for the end user operating a device connected to a network. It is not in itself an application, moreso the component within the application which controls the application's communication with other devices; masking the rest of the application from the data transmission, relying upon all the layers below to complete this process. The data is, at this stage, presented in a way the user can understand.  
*Remote login to hosts: Telnet  
*File transfer: File Transfer Protocol (FTP), Trivial File Transfer Protocol (TFTP)  
*Electronic mail transport: Simple Mail Transfer Protocol (SMTP)  
*Networking support: Domain Name System (DNS)  
*Host initialization: BOOTP  
*Remote host management: Simple Network Management Protocol (SNMP), Common Management Information Protocol over TCP (CMOT)  

<h2>Layer 6: Presentation layer</h2>
The presentation layer ensures the information that the application layer of one system sends out is readable by the application layer of another system. For example, a PC program communicates with another computer, one using extended binary coded decimal interchange code (EBCDIC) and the other using ASCII to represent the same characters. If necessary, the presentation layer might be able to translate between multiple data formats by using a common format.  

<h2>Layer 5: Session layer</h2>
This layer establishes request/response communication. When needed a session is started with authentication, after which a request is sent. After a response the session might be ended or a new request is sent. This is the first layer where a client/server concept is introduced. Where a specific device might change the role from client to server or vice versa. Essentially, the session layer responds to service requests from the presentation layer and issues service requests to the transport layer.  

<h2>Layer 4: Transport layer</h2>
The Transport Layer is the level at which system reliability and quality are ensured. This layer manages traffic flow through the network layer to reduce congestion on a network, and performs error checking ensuring quality of service by resending data when data has been corrupted (Think TCP/IP). Some of the most popular methods of encryption and firewall security take place on this layer. The protocols of this layer provide host-to-host communication services for applications.  

<h2>Layer 3: Network layer</h2>
The Network layer provides the means of transferring variable-length network packets from a source to a destination host via one or more networks. Within the service layering semantics of the OSI network architecture, the network layer responds to service requests from the transport layer and issues service requests to the data link layer. The Network (or routing) Layer works to coordinate related parts of a data conversation to ensure that large files are transferred. In other words, while the data link layer deals with the method in which the physical layer is used to transfer data, the network layer deals with organizing that data for transfer and reassembly. This layer also handles aspects of Routing Protocols, finding the available [best] path(s) from one network to another to ensure delivery of the data. 

<h2>Layer 2: Data link layer</h2>
This layer is the protocol layer that transfers data between adjacent network nodes in a wide area network (WAN) or between nodes on the same local area network (LAN) segment. The Data Layer is mainly the method in which information from the network is broken down into frames and transmitted over the physical layer. This layer is also responsible for some Error detection and correction and some addressing so different devices can tell each other apart in larger systems. Examples of data link protocols are Ethernet for local area networks (multi-node), the Point-to-Point Protocol (PPP), HDLC and ADCCP for point-to-point (dual-node) connections. In the Internet Protocol Suite (TCP/IP), the data link layer functionality is contained within the link layer, the lowest layer of the descriptive model, which also includes the functionality encompassed in the OSI model's physical layer.  

<h2>Layer 1: Physical layer</h2>
The physical layer translates logical communications requests from the data link layer into hardware-specific operations to cause transmission or reception of electronic signals. The physical layer refers to electrical and physical aspects of devices. In particular, it specifies how a device sends and receives information, such as using copper wires or fiber-optic cables. Examples of this include Ethernet or fiber optic cables, phone cords used for dial-up or DSL services, the coaxial cable used to provide broadband internet, the wires used to connect various components of a computer or even the radio signals used in wireless communication. Other functions of the physical layer include the conversion of signals into something that another layer can use (referred to as a bit), and adjusting the signal to allow for multiple users to use the same connections. 

<h3>Sources and further reading:</h3>
[Wikipedia - OSI Model](https://en.wikipedia.org/wiki/OSI_model#Protocol_layers_(Brief))  
[CWNP - Why Does the OSI Reference Model Matter?](https://www.cwnp.com/cwnp-wifi-blog/whytheosimodelmatters/)  

Powered by [Jekyll](http://jekyllrb.com)
