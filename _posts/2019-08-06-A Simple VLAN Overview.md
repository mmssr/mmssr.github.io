---
layout: post
title: "A Simple VLAN Overview"
date: 2019-08-06
---

VLANs, or virtual local area networks, are logical broadcast subdomains created within a physical LAN. While LANs are frequently comprised of multiple switches in order to connect multitudes of devices together, a VLAN can reduce congestion and ease physical configuration without use of separate switch infrastructures or multiple routers. Since a given VLAN represents its own broadcast domain, broadcasts sent by one of the VLAN members will only reach the other members of the VLAN, and broadcasts sent by those who are not a member of the VLAN will not reach the VLAN members. Additionally, since they are logically (rather than physically) implemented, if a VLAN workstation member is physically moved, say to a different floor within a company, the configuration to keep them within the same VLAN is much easier than the configuration to keep them within the same LAN segment.  
<hr>
<h2>Port-Based VLANs</h2>  
The simplest method is by configuring a switch to have a port-based VLAN. Each port on the switch is simply set up to contain a VLAN ID, which can be shared, like so:  

| Switch 1 Port | VLAN |  Device  |  
|------|------|----------|  
| 1    | 1    | PC       |  
| 2    | 1    | PC       |  
| 3    | 1    | Printer  |  
| 4    | 1    | Switch 2 |  
| 5    | 2    | PC       |  
| 6    | 2    | PC       |  
| 7    | 2    | PC       |  
| 8    | 2    | Switch 2 |  

With a configuration like this, the devices on VLAN 1 could communicate with one another, and the devices on VLAN 2 can communicate with one another, but the segmentation ensures that devices on VLAN 2 cannot use, say, the printer on VLAN 1. This method is usually easiest if the VLAN member needs only to communicate on one VLAN. If another switch was connected to the above port switch, like below, those devices could also connect with one another as though on the same VLAN. We could even have a 3rd VLAN as needed:  

| Switch 2 Port | VLAN |  Device  |  
|------|------|----------|  
| 1    | 1    | PC       |  
| 2    | 1    | PC       |  
| 3    | 1    | PC       |  
| 4    | 1    | Switch 1 |  
| 5    | 3    | PC       |  
| 6    | 2    | PC       |  
| 7    | 2    | PC       |  
| 8    | 2    | Switch 1 |  

<hr>  
<h2>Tagged VLAN</h2>
The second method is known as a tagged VLAN. This is accomplished by adding a VLAN tag to the Ethernet frame immediately after the MAC address. With frame based communication, less cables can be used than the previous method if each switch is capable of tagged VLANs, as only a single cable must connect the switches versus one per VLAN with a port-based VLAN. This method is more likely to be used if a VLAN member needs to be able to communicate on multiple networks.  

Powered by [Jekyll](http://jekyllrb.com)
