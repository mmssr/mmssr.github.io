---
layout: post
title: "Simple Subnet Calculations in IPv4"
date: 2019-07-05
---

Subnetting is the product of logically dividing up a network up into smaller, modular networks. This has many benefits, such as speed increases, easier administration, and increased options for maintaining security. While this post will not go into detail on the benefits or implementation of subnetworks, it is important to be able to calculate subnetwork ranges, network addresses, and broadcast addresses. In other words, "What is the network for for IP ```XXX.XXX.XXX.XXX/XX```? What is the IP range of this network?", and so on.
<hr> 
<h2>Finding the Subnet Mask</h2>  
When running commands such as ```ipconfig``` or ```ifconfig``` you may be given your IP and subnet mask; but let's assume that we are working with a classless IP displayed as ```192.168.0.23/21``` and that you have to calculate the subnet mask yourself. The portion preceding the "/" would be the IP, and the portion afterwards indicates the amount of subnet mask bits. Like IPs, subnet masks are comprised of four binary octets. The bits are typically assigned left to right, so we would have a subnet mask of ```11111111.11111111.11111000.00000000```,  or ```255.255.248.000``` in decimal format. 
<hr>  
<h2>Simple Calculations with the Binary Method</h2>  
Now that we have our subnet mask, lets calculate the network address, which is the very first IP in a subnet range. This is done with a bitwise AND operation, so the IP needs to be represented as binary as well, ```11000000.10101000.00000000.00010111```.  
Here we perform the AND operation and find our network address:  
```  

11000000.10101000.00000000.00010111 = 192.168.000.023 (Address)  
11111111.11111111.11111000.00000000 = 255.255.248.000 (Mask)  
-----------------------------------AND---------------  
11000000.10101000.00000000.00000000 = 192.168.000.000 (Network Address)

```  
This means that, for IP ```192.168.0.23/21```, our host ```0.23``` belongs to network ```192.168.0.0```. Now, lets look at the range of this network. We calculate this by once again looking at the bits assigned in our network mask octets, and keeping in mind the binary values of each. More specifically, we look at whichever our rightmost octet is which contains ```1``` values, and then the value of our rightmost bit.  

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

The rightmost bit corresponds to 8, which indicates our range. In other words, the subnet range that ```192.168.0.23/21``` belongs to is ```192.168.0.0``` through ```192.168.7.255```, and ```192.168.8.0``` is the adjascent subnet. Now we also know our broadcast address, which is always the last IP address in the range (```192.168.7.255```). Since the first and last IP addresses are reserved for the network and broadcast addresses, the host address range (all valid addressible IPs) would be ```192.168.0.1``` through ```192.168.7.254```, giving a total of 2,046 valid host addresses (calculated as (8 x 256) - 2). We can calculate the amount of valid host addresses as rightmost bit value, 8, multiplied by 256 per octet to the right, subtracting 2 lastly.  
<hr>  
<h2>Even Simpler Calculations with the Magic Number Method</h2>  
Now that we have examined the binary method, let's take a look at a simpler way of calculating the same subnet properties. Assuming we have our subnet mask, perhaps using a lookup chart such as the cheatsheet at the bottom, we instead want to find our leftmost number that isn't 255. Let's assume we are working with an IP of ```66.52.3.30``` and a subnet mask of ```255.255.255.240```. So, for ```255.255.255.240```, we have the number 240. To get our "magic number", we subtract 240 from 256, resulting in 16. Our magic number is calculated from the 4th binary octet, so we know our subnets calculate in intervals of 16 from the 4th octet. In other words, we know we would have network addresses of ```66.52.3.0```, followed by ```66.52.3.16```, followed by ```66.52.3.32```, and so on. Using this logic, we know that address ```66.52.3.30``` is within the network of ```66.52.3.16```, because the host range of the subnet extends from ```66.52.3.17``` through ```66.52.3.30```. We now also know that our broadcast address is ```66.52.3.31```, and that the next subnet range will begin at ```66.52.3.32```, giving a valid host range of 14. We can calculate the amount of valid host addresses as our magic number, 16, multiplied by 256 per octet to the right, subtracting 2 lastly.  
<hr>  
<h2>Cheatsheet Chart</h2>  

| Mask Bit Suffix | Subnet Mask | Host Addresses per Subnet |  
|-----------------|-----------------|---------------:|  
| /1 | 128.0.0.0 | 2,147,483,646 |  
| /2 | 192.0.0.0 | 1,073,741,822 |  
| /3 | 224.0.0.0 | 536,870,910 |  
| /4 | 240.0.0.0 | 268,435,454 |  
| /5 | 248.0.0.0 | 134,217,726 |  
| /6 | 252.0.0.0 | 67,108,862 |  
| /7 | 254.0.0.0 | 33,554,430 |  
| /8 | 255.0.0.0 | 16,777,214 |  
| /9 | 255.128.0.0 | 8,388,606 |  
| /10 | 255.192.0.0 | 4,194,302 |  
| /11 | 255.224.0.0 | 2,097,150 |  
| /12 | 255.240.0.0 | 1,048,574 |  
| /13 | 255.248.0.0 | 524,286 |  
| /14 | 255.252.0.0 | 262,142 |  
| /15 | 255.254.0.0 | 131,070 |  
| /16 | 255.255.0.0 | 65,534 |  
| /17 | 255.255.128.0 | 32,766 |  
| /18 | 255.255.192.0 | 16,382 |  
| /19 | 255.255.224.0 | 8,190 |  
| /20 | 255.255.240.0 | 4,094 |  
| /21 | 255.255.248.0 | 2,046 |  
| /22 | 255.255.252.0 | 1,022 |  
| /23 | 255.255.254.0 | 510 |  
| /24 | 255.255.255.0 | 254 |  
| /25 | 255.255.255.128 | 126 |  
| /26 | 255.255.255.192 | 62 |  
| /27 | 255.255.255.224 | 30 |  
| /28 | 255.255.255.240 | 14 |  
| /29 | 255.255.255.248 | 6 |  
| /30 | 255.255.255.252 | 2 |  
| /31 | 255.255.255.254 | x |  
| /32 | 255.255.255.255 | x |

Note that ```/31``` and ```/32``` are not frequently used, and would only be considered "valid" in hyper-specific circumstances which fall outside the scope of this article.  

Powered by [Jekyll](http://jekyllrb.com)
