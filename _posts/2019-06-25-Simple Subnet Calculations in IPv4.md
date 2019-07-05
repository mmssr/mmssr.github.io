---
layout: post
title: "Simple Subnet Calculations in IPv4"
date: 2019-06-25
---

Subnetting is the product of logically dividing up a network up into smaller, modular networks. This has many benefits, such as speed increases, easier administration, and increased options for maintaining security. While this post will not go into detail on the benefits or implementation of subnetworks, it is important to be able to calculate subnetwork ranges, network addresses, and broadcast addresses. In other words, "What is the network for for IP ```XXX.XXX.XXX.XXX/XX```? What is the IP range of this network?", and so on.
<hr> 
<h2>Finding the Subnet Mask</h2>  
When running commands such as ```ipconfig``` or ```ifconfig``` you may be given your IP and subnet mask, but lets assume that we are working with a classless IP displayed as ```192.168.0.23/21``` and that you have to calculate the subnet mask yourself. The portion preceeding the "/" would be the IP, and the portion afterwards indicates the amount of subnet mask bits. Like IPs, subnet masks are comprised of four binary octets. The bits are typically assigned left to right, so we would have a subnet mask of ```11111111.11111111.11111000.00000000```,  or ```255.255.248.000``` in decimal format. 
<hr>  
<h2>Simple Calculations with the Binary Method</h2>  
Now that we have our subnet mask, lets calculate the network address, which is the very first IP in a subnet range. This is done with a bitwise AND operation, so the IP needs to be represented as binary as well, ```11000000.10101000.00000000.00010111```.  
Here we perform the AND operation:  

```    11000000.10101000.00000000.00010111```  
```AND 11111111.11111111.11111000.00000000```    
```---------------------------------------```  
```    11000000.10101000.00000000.00000000```  
  
Resulting in a network address of ```192.168.0.0```. This means that, for ```IP 192.168.0.23```, our host ```0.23``` belongs to network ```192.168.0.0```. Now, lets look at the range of this network. We calculate this by once again looking at the bits assigned in our network mask octets, and keeping in mind the binary values of each. More specifically, we look at whichever our rightmost octet is which contains ```1``` values.  

<table>
  <tr>
    <th>Value:</th>
    <td>128</td>
    <td>64</td>
    <td>32</td>
    <td>16</td>
    <td>8</td>
    <td>4</td>
    <td>2</td>
    <td>1</td>
  </tr>
  <tr>
    <th>Bits:</th>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
</table>  

TODO: describe how subnetting works at a binary level, followed by how subnetting works using the "magic number" method. List sources.

Powered by [Jekyll](http://jekyllrb.com)
