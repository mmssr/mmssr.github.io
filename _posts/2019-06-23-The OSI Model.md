---
layout: post
title: "The OSI Model"
date: 2019-06-23
---

The Open Systems Interconnection (OSI) model is a method used to describe computer networking. The OSI model creates a common description for how networks function, which is necesary for understanding and discussion documentation as well as functionality. Each layer can be optimized and hardened, and allows for abstraction./

| Layer | Name | Description |Technologies|Data Unit|Layer Type|  
|-------|-------|-------|-------|-------|-------|  
| 7 | Application | Network process to computer programs| FTP, HTTP, SMTP | Data | Host |  
| 6 | Presentation | Data representation, security encryption, convert computer code to network formatted code | AFP, MIME | Data| Host  |  
| 5 | Session | Interhost communication, managing sessions between programs | X255, SCP | Data | Host |  
| 4 | Transport | End-to-end connections, reliability and flow control| TCP | Segment | Host |  
| 3 | Network | Path determination and logical addressing | IP | Packet | Media |  
| 2 | Data Link | Physical addressing | ARP, PPP | Frame | Media |  
| 1 | Physical | The physical infrastructure used to send and receive signals | USB, Bluetooth  | Bit | Media |  

<h1>Layer 7: Application layer<h1/>/
The application layer provides an interface for the end user operating a device connected to a network. This layer is what the user sees, in terms of loading an application (such as Web browser or e-mail); that is, this application layer is the data the user views while using these applications. Examples of application layer functionality include:
*Support for file transfers
*Ability to print on a network
*Electronic mail
*Electronic messaging
*Browsing the World Wide Web. 

<h1>Layer 6: Presentation layer<h1/>/
To be able to properly interpret a message sent through the network this layer is responsible for the proper translation or interpretation. 

<h1>Layer 5: Session layer<h1/>/
This layer establishes request/response communication. When needed a session is started with authentication, after which a request is sent. After a response the session might be ended or a new request is sent. This is the first layer where a client/server concept is introduced. Where a specific device might change the role from client to server or vice versa. 

<h1>Layer 4: Transport layer<h1/>/
The Transport Layer is the level at which system reliability and quality are ensured. This layer manages traffic flow through the network layer to reduce congestion on a network, and performs error checking ensuring quality of service by resending data when data has been corrupted. Some of the most popular methods of encryption and firewall security take place on this layer. 

<h1>Layer 3: Network layer<h1/>/
The Routing Layer works to coordinate related parts of a data conversation to ensure that large files are transferred. In other words, while the data link layer deals with the method in which the physical layer is used to transfer data, the network layer deals with organizing that data for transfer and reassembly. This layer also handles aspects of Routing Protocols, finding the available [best] path(s) from one network to another to ensure delivery of the data. 

<h1>Layer 2: Data link layer<h1/>/
The Data Layer is mainly the method in which information from the network is broken down into frames and transmitted over the physical layer. This layer is also responsible for some Error detection and correction and some addressing so different devices can tell each other apart in larger systems. 

<h1>Layer 1: Physical layer<h1/>/
The physical layer refers to electrical and physical aspects of devices. In particular, it specifies how a device sends and receives information, such as using copper wires or fiber-optic cables. Examples of this include Ethernet or fiber optic cables, phone cords used for dial-up or DSL services, the coaxial cable used to provide broadband internet, the wires used to connect various components of a computer or even the radio signals used in wireless communication. Other functions of the physical layer include the conversion of signals into something that another layer can use (referred to as a bit), and adjusting the signal to allow for multiple users to use the same connections. 

Sources and further reading:/ 
[Simple Wikipedia - OSI Model](https://simple.wikipedia.org/wiki/OSI_model)/
[CWNP - Why Does the OSI Reference Model Matter?](https://www.cwnp.com/cwnp-wifi-blog/whytheosimodelmatters/)/

Powered by [Jekyll](http://jekyllrb.com)
