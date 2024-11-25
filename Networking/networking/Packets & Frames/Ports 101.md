# Ports 101

Networking devices use ports to enforce strict rules when communicating with one another. When a connection has been established, any data sent or received by a device will be sent through these ports. In computing, ports are a numerical value between 0 and 65535 (65,535).

Because ports can range from anywhere between 0 - 65535, there quickly runs the risk of losing track of what application is using what port. Because of that, applications, software and behaviors are associated with a strict set of rules. For example, by enforcing that any web browser data is sent over port 80, software developers can design a web browser such as Google Chrome or Firefox to interpret the data the same way as one another.

This means that all web browsers now share one common rule: data is sent over port 80. How the browsers look  feel and ease to use is up to the designer or the user's decision.

While the standard rule for web data is port 80, a few other protocols have been allocated a standard rule. Any port that is within 0 and 1024 is known as a common port. Some of these protocols are as follows:

| **Protocol** | **Port Number** | **Description** |
| --- | --- | --- |
| **F**ile **T**ransfer **P**rotocol (**<span style="color: inherit;">FTP</span>**) | 21  | This protocol is used by a file-sharing application built on a client-server model, meaning you can download files from a central location. |
| **S**ecure **Sh**ell (**<span style="color: inherit;">SSH</span>**) | 22  | This protocol is used to securely login to systems via a text-based interface for management. |
| **H**yper**T**ext Transfer Protocol (**<span style="color: inherit;">HTTP</span>**) | 80  | This protocol powers the World Wide Web (WWW)! Your browser uses this to download text, images and videos of web pages. |
| **H**yper**T**ext **T**ransfer **P**rotocol **S**ecure (**HTTPS**) | 443 | This protocol does the exact same as above; however, securely using encryption. |
| **S**erver **M**essage **B**lock (**<span style="color: inherit;">SMB</span>**) | 445 | This protocol is similar to the File Transfer Protocol (<span style="color: inherit;">FTP</span>); however, as well as files, <span style="color: inherit;">SMB</span> allows you to share devices like printers. |
| **R**emote **D**esktop **P**rotocol (**<span style="color: inherit;">RDP</span>**) | 3389 | This protocol is a secure means of logging in to a system using a visual desktop interface (as opposed to the text-based limitations of the <span style="color: inherit;">SSH</span> protocol). |

We have only briefly covered the more common protocols in cybersecurity. You can find a [table of the 1024 common ports](http://www.vmaxx.net/techinfo/ports.htm) listed for more information.

What is worth noting is that these protocols only follow the standards, i.e. you can administer applications that interact with these protocols on a different port other than what is the standard (running a web server on 8080 instead of the 80 standard port\]). Note however, applications will presume that the standard is being followed, so you will have to provide a colon (:) along with the port number.

&nbsp;