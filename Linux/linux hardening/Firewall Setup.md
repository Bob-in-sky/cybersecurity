# Firewall Setup

The primary goal of <span style="color: #2dc26b;">firewalls</span> is to provide a security mechanism for controlling and monitoring network traffic between different network segments, such as internal and external networks or different network zones. Firewalls play a crucial role in protecting computer networks from unauthorized access, malicious traffic, and other security threats. Linux, being a popular operating system used in servers and other network devices, provides built-in firewall capabilities that can be used to control network traffic. In other words, they can filter incoming and outgoing traffic based on pre-defined rules, protocols, ports, and other criteria to prevent unauthorized access and mitigate security threats. The specific goal of a firewall implementation can vary depending on the specific needs of the organization, such as ensuring the confidentiality, integrity, and availability of network resources.

An example from the history of Linux firewalls is the development of the <span style="color: #2dc26b;">iptables</span> tool, which replaced the earlier <span style="color: #2dc26b;">ipchains</span> and <span style="color: #2dc26b;">ipfwadm</span> tools. The <span style="color: #2dc26b;">iptables</span> utility was first introduced in the Linux 2.4 kernel in 2000 and provided a flexible and efficient mechanism for filtering network traffic. Iptables became the de facto standard firewall solution for Linux systems, and it has been widely adopted by many organizations and users.

The <span style="color: #2dc26b;">iptable</span> utility provided a simple yet powerful command-line interface for configuring firewall rules, which could be used to filter traffic based on various criteria such as IP addresses, ports, protocols, and more. <span style="color: #2dc26b;">Iptables</span> was designed to be highly customizable and could be used to create complex firewall rulesets that could protect against various security threats such as <span style="color: #2dc26b;">denial-of-service</span> (<span style="color: #2dc26b;">DoS</span>) attacks, port scans, and network intrusion attempts.

In Linux, the firewall functionality is typically implemented using the <span style="color: #2dc26b;">Netfilter</span> framework, which is an integral part of the kernel. Netfilter provides a set of hooks that can be used to intercept and modify network traffic as it passes through the system. The <span style="color: #2dc26b;">iptables</span> utility is commonly used to configure the firewall rules on Linux systems.

&nbsp;

## iptables

The <span style="color: #2dc26b;">iptables</span> utility provides a flexible set of rules for filtering network traffic based on various criteria such as source and destination IP addresses, port numbers, protocols, and more. There also exist other solutions like <span style="color: #2dc26b;">nftables</span>, <span style="color: #2dc26b;">UFW</span>, and <span style="color: #2dc26b;">FirewallD</span>.

<span style="color: #2dc26b;">Nftables</span> provides a more modern syntax and improved performance over <span style="color: #2dc26b;">iptables</span>. However, the syntax of <span style="color: #2dc26b;">nftables</span> rules is not compatible with <span style="color: #2dc26b;">iptables</span>, so migration to <span style="color: #2dc26b;">nftables</span> requires some effort.

<span style="color: #2dc26b;">UFW</span> stands for "Uncomplicated Firewall" and provides a simple yet user-friendly interface for configuring firewall rules. <span style="color: #2dc26b;">UFW</span> is built on top of the <span style="color: #2dc26b;">iptables</span> framework, like <span style="color: #2dc26b;">nftables</span>, and provides an easier way to manage firewall rules.

Finally, <span style="color: #2dc26b;">FirewallD</span> provides a dynamic and flexible firewall solution that can bbe used to manage complex firewall configurations, and it supports a rich set of rules for filtering network traffic and can be used to create custom firewall zones and services. It consists of several components that work together to provide a flexible and powerful firewall solution. The main components of <span style="color: #2dc26b;">iptables</span> are:

| Component | Description |
| --- | --- |
| <span style="color: #2dc26b;">Tables</span> | Tables are used to organize and categorize firewall rules |
| <span style="color: #2dc26b;">Chains</span> | Chains are used to group a set of firewall rules applied to a specific type of network traffic |
| <span style="color: #2dc26b;">Rules</span> | Rules define the criteria for filtering network traffic and the actions to take for packets that match the criteria |
| <span style="color: #2dc26b;">Matches</span> | Matches are used to match specific criteria for filtering network traffic, such as source or destination IP addresses, ports, protocols, and more |
| <span style="color: #2dc26b;">Targets</span> | Targets specify the action for packets that match a specific rule. For example, targets can be used to accept, drop, or reject packets or modify the packets in another way |

### Tables

When working with firewalls on Linux systems, it is important to understand how tables work in <span style="color: #2dc26b;">iptables</span>. Tables in <span style="color: #2dc26b;">iptables</span> are used to categorize and organize firewall rules based on the type of traffic that they are designed to handle. These tables are used to organize and categorize firewall rules. Each table is responsible for performing a specific set of tasks.

| Table Name | Description | Built-in Chains |
| --- | --- | --- |
| <span style="color: #2dc26b;">filter</span> | Used to filter network traffic based on IP addresses, ports, and protocols | INPUT, OUTPUT, FORWARD |
| <span style="color: #2dc26b;">nat</span> | Used to modify the source of destination IP addresses of network packets | PREROUTING, POSTROUTING |
| <span style="color: #2dc26b;">mangle</span> | Used to modify the header fields of network packets | PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING |

In addition to the built-in tables, <span style="color: #2dc26b;">iptables</span> provides a fourth table called the raw table, which is used to configure special packets processing options. The raw table contains two built-in chains: <span style="color: #2dc26b;">PREROUTING</span> and <span style="color: #2dc26b;">OUTPUT</span>.

&nbsp;

### Chains

In <span style="color: #2dc26b;">iptables</span>, chains organize rules that define how network traffic should be filtered or modified. There are two types of chains in <span style="color: #2dc26b;">iptables</span>:

- <span style="color: #2dc26b;">Built-in Chains</span>
- <span style="color: #2dc26b;">User-defined Chains</span>

The built-in chains are pre-defined and automatically created when a table is created. Each table has a different set of built-in chains. For example, the filter table has three built-in chains:

- <span style="color: #2dc26b;">INPUT</span>
- <span style="color: #2dc26b;">OUTPUT</span>
- <span style="color: #2dc26b;">FORWARD</span>

These chains are used to filter incoming and outgoing network traffic, as well as traffic that is being forwarded between different network interfaces. The <span style="color: #2dc26b;">nat</span> table has two built-in chains:

- <span style="color: #2dc26b;">PREROUTING</span>
- <span style="color: #2dc26b;">POSTROUTING</span>

The <span style="color: #2dc26b;">PREROUTING</span> chain is used to modify the destination IP address of incoming packets before the routing table processes them. The <span style="color: #2dc26b;">POSTROUTING</span> chain is used to modify the source IP address of outgoing packets after the routing table has processed them. The mangle table has five built-in chains:

- <span style="color: #2dc26b;">PREROUTING</span>
- <span style="color: #2dc26b;">OUTPUT</span>
- <span style="color: #2dc26b;">INPUT</span>
- <span style="color: #2dc26b;">FORWARD</span>
- <span style="color: #2dc26b;">POSTROUTING</span>

These chains are used to modify the header fields of incoming and outgoing packets and packets being processed by the corresponding chains.

<span style="color: #2dc26b;">User-defined Chains</span> can simply rule management by grouping firewall rules based on specific criteria, such as source IP address, destination port, or protocol. They can be added to any of the three main tables. For example, if an organization has multiple web servers that all require similar firewall rules, the rules for each server could be grouped in a user-defined chain. Another example is when a user-defined chain could filter traffic destined for a specific port, such as port 80 (HTTP). The user could then add rules to this chain that specifically filter traffic destined for port 80.

&nbsp;

### Rules and Targets

<span style="color: #2dc26b;">Iptables</span> rules are used to define the criteria for filtering network traffic and the actions to take for packets that match the criteria. Rules are added to chains using the <span style="color: #2dc26b;">\-A</span> option followed by the chain name, and they can be modified or deleted using various other options.

Each rule consists of a set of criteria, or matches, and a target specifying the action for packets that match the criteria. The criteria, or matches, match specific fields in the IP header, such as the source or destination IP address, protocol, source, destination port number, and more. The target specifies the action for packets that match a specific rule. For example, targets can accept, drop, reject, or modify the packets. Some of the common targets used in <span style="color: #2dc26b;">iptables</span> rules include the following:

| Target Name | Description |
| --- | --- |
| <span style="color: #2dc26b;">ACCEPT</span> | Allows the packet to pass through the firewall and continue to its destination |
| <span style="color: #2dc26b;">DROP</span> | Drops the packet, effectively blocking it from passing through the firewall |
| <span style="color: #2dc26b;">REJECT</span> | Drops the packet and sends an error message back to the source address, notifying them that the packet was blocked |
| <span style="color: #2dc26b;">LOG</span> | Logs the packet information to the system log |
| <span style="color: #2dc26b;">SNAT</span> | Modifies the source IP address of the packet, typically used for Network Address Translation (NAT) to translate private IP addresses to public IP addresses |
| <span style="color: #2dc26b;">DNAT</span> | Modifies the destination IP address of the packet, typically used for NAT to forward traffic from one IP address to another |
| <span style="color: #2dc26b;">MASQUERADE</span> | Similar to SNAT but used when the source IP address is not fixed, such as in dynamic IP address scenario |
| <span style="color: #2dc26b;">REDIRECT</span> | Redirects packets to another port or IP address |
| <span style="color: #2dc26b;">MARK</span> | Adds or modifies the Netfilter mark value of the packet, which can be used for advanced routing or other purposes |

&nbsp;Let us illustrate a rule and consider the we want to add a new entry to the <span style="color: #2dc26b;">INPUT</span> chain that allows incoming TCP traffic on port 22 (SSH) to be accepted. The command for that would look like the following:

```Shell
bobinsson@BoB$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

&nbsp;

### Matches

<span style="color: #2dc26b;">Matches</span> are used to specify the criteria that determines whether a firewall rule should be applied to a particular packet or connection. Matches are used to match specific characteristics of network traffic, such as the source or destination IP address, protocol, port number, and more.

| **Match Name** | **Description** |
| --- | --- |
| <span style="color: #2dc26b;">\-p</span> or <span style="color: #2dc26b;">\--protocol</span> | Specifies the protocol to match (e.g. tcp, udp, icmp) |
| <span style="color: #2dc26b;">\--dport</span> | Specifies the destination port to match |
| <span style="color: #2dc26b;">\--sport</span> | Specifies the source port to match |
| <span style="color: #2dc26b;">\-s</span> or <span style="color: #2dc26b;">\--source</span> | Specifies the source IP address to match |
| <span style="color: #2dc26b;">\-d</span> or <span style="color: #2dc26b;">\--destination</span> | Specifies the destination IP address to match |
| <span style="color: #2dc26b;">\-m state</span> | Matches the state of a connection (e.g. NEW, ESTABLISHED, RELATED) |
| <span style="color: #2dc26b;">\-m multiport</span> | Matches multiple ports or port ranges |
| <span style="color: #2dc26b;">\-m tcp</span> | Matches TCP packets and includes additional TCP-specific options |
| <span style="color: #2dc26b;">\-m udp</span> | Matches UDP packets and includes additional UDP-specific options |
| <span style="color: #2dc26b;">\-m string</span> | Matches packets that contain a specific string |
| <span style="color: #2dc26b;">\-m limit</span> | Matches packets at a specified rate limit |
| <span style="color: #2dc26b;">\-m conntrack</span> | Matches packets based on their connection tracking information |
| <span style="color: #2dc26b;">\-m mark</span> | Matches packets based on their Netfilter mark value |
| <span style="color: #2dc26b;">\-m mac</span> | Matches packets based on their MAC address |
| <span style="color: #2dc26b;">\-m iprange</span> | Matches packets based on a range of IP addresses |

&nbsp;

In general, matches are specifiec using the "-m" option in iptables. For example, the following command adds a rule to the "INPUT" chain in the "filter" tbale that matches incoming TCP traffic on port 80:

```Shell
bobinsson@BoB$ sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

This example rule matches incoming TCP traffic (<span style="color: #2dc26b;">\-p tcp</span>) on port 80 (<span style="color: #2dc26b;">\--dport 80</span>) and jumps to the accept target (<span style="color: #2dc26b;">\-j ACCEPT</span>) if the match is successful.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# Practice

|     |
| --- |
| **Practice** |
| 1\. Launch a web server on TCP/8080 port on your target and use iptables to block incoming traffic on that port. |
| 2\. Change iptables rules to allow incoming traffic on the TCP/8080 port. |
| 3\. Block traffic from a specific IP address. |
| 4\. Allow traffic from a specific IP address. |
| 5\. Block traffic based on protocol. |
| 6\. Allow traffic based on protocol. |
| 7\. Create a new chain. |
| 8\. Forward traffic to a specific chain. |
| 9\. Delete a specific rule. |
| 10\. List all existing rules. |