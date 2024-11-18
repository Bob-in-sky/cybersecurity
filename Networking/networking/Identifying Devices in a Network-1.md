# Identifying Devices in a Network

To communicate and maintain order inside a network, devices must be both identifying and identifiable. Network devices have two means of identification, with one of them being permeable:

- Internet Protocol (IP) Address
- Media Access Control (MAC) Address

&nbsp;

## IP Addresses

An IP address can be used as a way o identifying a host on a network for a period of time, where that IP address can then be associated with another device without the IP address changing. The IP address is composed of:

| octet #1 | octet #2 | octet #3 | octet #4 |
| --- | --- | --- | --- |
| 192. | 168. | 1.  | 1   |
| 0 - 255 | 0 - 255 | 0 - 255 | 0 - 255 |

An IP address is a set of numbers that are divided into four octets. The value of each octet will summarize to be the IP address of the device on the network. This number is calculated through a technique knows as IP addressing & subnetting. What's important to understand is that IP addresses can change from device to device but cannot be active simultaneously more than once within the same network.

IP addresses follow a set of standards known as protocols. These protocols are the backbone of networking and force many devices to communicate in the same language. However, devices can be on both a private or a public network. Depending on where they are will determine what type of IP address they have assigned: a private or a public IP address.

A public address is used to identify the device on the internet, whereas a private address is used to identify a device amongst other devices. Take the table & screenshot below as example. Here we have two devices on a private network:

| **Device Name** | **IP Address** | **IP Address Type** |
| --- | --- | --- |
| DESKTOP-KJE57FD | 192.168.1.77 | Private |
| DESKTOP-KJE57FD | 86.157.52.21 | Public |
| CMNatic-PC | 192.168.1.74 | Private |
| CMNatic-PC | 86.157.52.21 | Public |

![img](../../_resources/1-1.png)

These two devices will be able to user their private IP addresses to communicate with each other. However, any data sent to the internet from either of these devices will be identified by the same public IP address. Public IP addresses are given by the Internet Service Provider (ISP) at a monthly fee.

![2.png](../../_resources/2-1.png)

As more and more devices become connected, it becomes increasingly difficult to get a public address that isn't already in use. For example, Cisco, an industry giant in the world of networking, estimated that there would be approximately 50 billion devices connected on the Internet by the end of 2021. (Cisco., 2021). Enter IP address versions. So far, we have only discussed one version of the Internet Protocol addressing scheme known as IPv4, which uses a numbering system of 2^32 IP addresses (4.29 billion) -- so you can see why there is such a shortage.

IPv6 is a new iteration of the Internet Protocol addressing scheme to help tackle this issue. Although it is seemingly more daunting, it boasts a few benefits:

- Supports up to 2^128 of IP addresses (340 trillion-plus), resolving the issues faced with IPv4
- More efficient due to new methodologies

<img src="../../_resources/ipv6-1.png" alt="ipv6.png" width="649" height="156" style="display: block; margin: 0 auto;" class="jop-noMdConv">

&nbsp;

## MAC Address

Devices on a network will all have a physical network interface, which is a microchip board found on the device's motherboard. This network interface is assigned a unique address at the factory it was built, called a Media Access Control (MAC) address. The MAC addresses is a twelve-character hexadecimal number split into two's and separated by a colon. These colons are considered separators. For example, a4:c3:f0:85:ac:2d. The first six characters represent the company that made the network interface, and the last six is a unique number.

<img src="../../_resources/394caee97fb1b9f7b5a5f7a7ea0a9f71-1.png" alt="394caee97fb1b9f7b5a5f7a7ea0a9f71.png" width="508" height="298" style="display: block; margin: 0 auto;" class="jop-noMdConv">

However, an interesting thing with MAC addresses is that they can be faked or <span style="color: #2dc26b;">spoofed</span> in a process known as <span style="color: #2dc26b;">spoofing</span>. This <span style="color: #2dc26b;">spoofing</span> occurs when a networked device pretends to identify as another using it's MAC address. When this occurs, it can often break poorly implemented security designs that assume that devices talking on a network are trustworthy. Take the following scenario: A firewall is configured to allow any communication going to and from the MAC address of the administrator. If a device were to pretend or <span style="color: #2dc26b;">spoof</span> this MAC address, the firewall would now think that it is receiving communication from the administrator when it isn't.

Places such as cafes, coffee shops, and hotels alike often use MAC address control when using their "Guest "or "Public" Wi-Fi. This configuration could offer better services, i.e. a faster connection for a price if you are willing to pay the fee per device.