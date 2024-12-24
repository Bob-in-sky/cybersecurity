# OSI Model

The OSI model (Open Systems Interconnection Model) is an absolute fundamental model used in networking. This critical model provides a framework dictating how all network devices will send, receive and interpret data.

<img src="../../_resources/6d17472b87f8792dadde3bb06aa1fdaa.svg" alt="6d17472b87f8792dadde3bb06aa1fdaa.svg" width="465" height="345" class="jop-noMdConv" style="display: block; margin: 0 auto;">

The OSI (Open Systems Interconnection) model is a conceptual model developed by the International Organization for Standardization (ISO) that describes how communications should occur in a computer network. In other words, the OSI model defines a framework for computer network communications. Although this model is theoretical, it is vital to learn and understand as it helps grasp networking concepts on a deeper level.

One of the main benefits of the OSI model is that devices can have different functions and designs on a network while communicating with other devices. Data sent across a network that follows the uniformity of the OSI model can be understood by other devices.

The OSI model consists of seven layers, each has a different set of responsibilities and is arranged from Layer 7 to Layer 1.

At every individual layer that data travels through, specific processes take place, and pieces of information are added to this data, this process is called encapsulation.

| Layer Number | Layer Name | Main Function | Example Protocols and Standards |
| --- | --- | --- | --- |
| Layer 7 | Application layer | Providing services and interfaces to applications | HTTP, FTP, DNS, POP3, <span style="color: inherit;">SMTP</span>, <span style="color: inherit;">IMAP</span> |
| Layer 6 | Presentation layer | Data encoding, encryption, and compression | Unicode, <span style="color: inherit;">MIME</span>, JPEG, PNG, MPEG |
| Layer 5 | Session layer | Establishing, maintaining, and synchronising sessions | NFS, RPC |
| Layer 4 | Transport layer | End-to-end communication and data segmentation | <span style="color: inherit;">UDP</span>, <span style="color: inherit;">TCP</span> |
| Layer 3 | Network layer | Logical addressing and routing between networks | IP, ICMP, IPSec |
| Layer 2 | Data link layer | Reliable data transfer between adjacent nodes | Ethernet (802.3), WiFi (802.11) |
| Layer 1 | Physical layer | Physical data transmission media | Electrical, optical, and wireless signals |