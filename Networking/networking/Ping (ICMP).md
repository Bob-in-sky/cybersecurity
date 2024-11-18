# Ping (ICMP)

Ping is one of the most fundamental network tools available. Ping uses Internet Control Message Protocol (ICMP) packets to define the performance of a connection between devices, for example, if the connection exists or is reliable.

The time taken for ICMP packets travelling between devices is measured by ping, such as in the example below. This measurement is done using ICMP's echo packet and then ICMP's echo reply from the target device.

Pings can be performed  against devices in a network, such as your home network or resources like websites. This tool can be easily used and comes installed on Operating Systems (OS) such as Linux and Windows. The syntax to do a simple ping is ping <span style="color: #2dc26b;">\[IP address or website URL\]</span>.

```Shell
┌──(bobinsson㉿BoB)-[~]
└─$ ping 192.168.1.254
PING 192.168.1.254 (192.168.1.254) 56(84) bytes of data.
64 bytes from 192.168.1.254: icmp_seq=1 ttl=113 time=27.7 ms
64 bytes from 192.168.1.254: icmp_seq=2 ttl=113 time=20.5 ms
64 bytes from 192.168.1.254: icmp_seq=3 ttl=113 time=16.5 ms
64 bytes from 192.168.1.254: icmp_seq=4 ttl=113 time=17.9 ms
64 bytes from 192.168.1.254: icmp_seq=5 ttl=113 time=15.7 ms
64 bytes from 192.168.1.254: icmp_seq=6 ttl=113 time=14.6 ms
^C
--- 192.168.1.254 statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5010ms
rtt min/avg/max/mdev = 14.577/18.794/27.650/4.371 ms
```

Here we are pinging a device that has the private address 192.168.1.254, ping inform us that we have sent sic ICMP packets, all of which were received with an average time of 18.794 milliseconds.