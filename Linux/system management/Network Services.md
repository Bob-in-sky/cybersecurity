# Network Services

When working with Linux, it is necessary to deal with different network services. The competence to work with these services is essential for communicating with other computers over the network, connect, transfer files, analyze network traffic, and learn how to configure such services to identify potential vulnerabilities in penetration tests. \]this knowledge also pushes our understanding of network security as we learn what options each service and its configuration entails.

During penetration testing, we come across Linux hosts to be probed for vulnerabilities. Listening to the network, it is possible to see when a user from that host connects to another server via an unencrypted FTP server, so it is viable to detect the credentials of the user in clear text. Of course, the likelihood of this scenario occurring would be much lower if the user knew that FTP does not encrypt the connections and the data sent. And as a Linux administrator, this could be a fatal error, as it tells a lot not only about the security measures on the network, but also about the administrators who are responsible for the security of their network.

We will not be able to cover all network services, but we will focus on and cover the most important ones. Because not only from the perspective of an administrator and user, it is of great benefit but also as a penetration tester for the interaction between other hosts and our machine.

&nbsp;

## SSH

Secure Shell (<span style="color: #2dc26b;">SSH</span>) is a network protocol that allows the secure transmission of data and commands over a network. It is widely used to securely manage remote systems and securely access remote systems to execute commands or transfer files. In order to connect to a remote host via SSH, a corresponding SSH server must be available and running.

The most commonly used SSH server is the OpenSSH server. OpenSSH is a free and open-source implementation of the Secure Shell (SSH)  protocol that allows the secure transmission of data and commands over a network.

Administrators use OpenSSH to securely manage remote systems by establishing an encrypted connection to a remote host. With OpenSSH, administrators can execute commands on remote systems, securely transfer files, and establish a secure remote connection without the transmission of data and commands being intercepted by third parties.

### Install OpenSSH

```Shell
bobinsson@BoB$ sudo apt install openssh-server -y
```

To check if the server is running, use the following command:

```Shell
bobinsson@BoB$ systemctl status ssh

● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/system/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2023-02-12 21:15:27 GMT; 1min 22s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 7740 (sshd)
      Tasks: 1 (limit: 9458)
     Memory: 2.5M
        CPU: 236ms
     CGroup: /system.slice/ssh.service
             └─7740 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```

### Logging In

As penetration tester, OpenSSH can be used to securely access remote systems when performing a network audit. To do this, we can use the following command:

```Shell
bobinsson@BoB$ ssh <remote_username>@<remote_host_ip> [<port>]

The authenticity of host '<remote_host_ip> (<remote_host_ip>)' can't be established.
ECDSA key fingerprint is SHA256:bKzhv+n2pYqr2r...Egf8LfqaHNxk.

Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '<remote_host_ip>' (ECDSA) to the list of known hosts.

<remote_username>@<remote_host_ip>'s password: ***********
```

OpenSSH can be configured and customized by editing the file <span style="color: #2dc26b;">/etc/ssh/sshd_config</span> with a text editor. Here it's possible to adjust settings such as the maximum number of concurrent connections, the use of passwords or keys for logins, host key checking, and more. However, it is important to note that changes to the OpenSSH configuration file must be done carefully.

For example, we can use SSH to securely log in to a remote system and execute commands or use tunneling and port forwarding to tunnel data over an encrypted connection to verify network settings and other system settings, without the possibility of third parties intercepting the transmission of data commands.

&nbsp;

## NFS

Network File System (<span style="color: #2dc26b;">NFS</span>) is a network protocol that allows us to store and manage files on remote systems as if they were stored on the local system. It enables easy and efficient management of files across networks. For example, administrators use NFS to store and manage files centrally (for Linux and Windows systems) to enable easy collaboration and management of data. For linux, there are several NFS servers, including NFS-UTILS (<span style="color: #2dc26b;">Ubuntu</span>), NFS-Ganesha (<span style="color: #2dc26b;">Solaris</span>), and OpenNFS (<span style="color: #2dc26b;">Redhat Linux</span>).

It can be used to share and manage resources efficiently, e.g., to replicate file systems between servers. It also offers features such as access controls, real-time file transfer, and support for multiple users accessing data simultaneously. We can use this service just like FTP in case there is no FTP client installed on the target system, or NFS is running instead of FTP.

### Install NFS

NFS can be installed on Linux with the following command:

```Shell
bobinsson@BoB$ sudo apt install nfs-kernel-server -y
```

### Server Status

The following command can be used to check if the server is running:

```Shell
bobinsson@BoB$ systemctl status nfs-kernel-server

● nfs-server.service - NFS server and services
     Loaded: loaded (/lib/system/system/nfs-server.service; enabled; vendor preset: enabled)
     Active: active (exited) since Sun 2023-02-12 21:35:17 GMT; 13s ago
    Process: 9234 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
    Process: 9235 ExecStart=/usr/sbin/rpc.nfsd $RPCNFSDARGS (code=exited, status=0/SUCCESS)
   Main PID: 9235 (code=exited, status=0/SUCCESS)
        CPU: 10ms
```

NFS can be configured via the configuration file <span style="color: #2dc26b;">/etc/exports</span>. This file specifies which directories should be shared and the access rights for users and systems. It is also possible to configure settings such as the transfer speed and the use of encryption. NFS access rights determine which users and systems can access the shared directories and what actions they can perform. Here are some important access rights that can be configured via NFS:

| Permissions | Description |
| --- | --- |
| <span style="color: #2dc26b;">rw</span> | Gives users and systems read and write permissions to the shared directory |
| <span style="color: #2dc26b;">ro</span> | Gives users and systems read-only access to the shared directory |
| <span style="color: #2dc26b;">no_root_squash</span> | Prevents the root user on the client from being restricted to the rights of a normal user |
| <span style="color: #2dc26b;">root_squash</span> | Restricts the rights of the root user on the client to the rights of a normal user |
| <span style="color: #2dc26b;">sync</span> | Synchronizes the transfer of data to ensure that changes are only transferred after they have been saved on the file system |
| <span style="color: #2dc26b;">async</span> | Transfers data asynchronously, which makes the transfer faster, but may cause inconsistencies in the file system if changes have not been fully committed |

&nbsp;

For example, a new folder can be created and shared temporarily in NFS by doing:

### Create NFS Share

```Shell
bobinsson@BoB$ mkdir nfs_sharing
bobinsson@BoB$ echo '/home/bobinsson/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
cat /etc/exports | grep -V "#"

/home/bobinsson/nfs_sharing hostname(rw,sync,no_root_squash)
```

To create an  NFS share and work with it on the target system, it's necessary to mount it first with the following command:

### Mount NFS Share

```Shell
bobinsson@BoB$ mkdir ~/target_nfs
bobinsson@BoB$ mount <target_ip>:/home/user/scripts ~/target_nfs
bobinsson@BoB$ tree ~/target_nfs

target_nfs/
├── css.css
├── html.html
├── javascript.js
├── php.php
└── xml.xml

0 directories, 5 files
```

So we have mounted the NFS share  (<span style="color: #2dc26b;">dev_scripts</span>) from our target (<span style="color: #2dc26b;">&lt;target_ip&gt;</span>) locally to our system in the mount point <span style="color: #2dc26b;">target_nfs</span> over the network and can view the contents just as if we were on the target system. There are even some methods that can be used in specific cases to escalate our privileges on the remote system using NFS.

&nbsp;

## Web Server

As penetration testers, we need to understand how web servers work because they are a critical part of web applications and often serve as targets for us to attack. A web server is a type of software that provides data and documents or other applications and functions over the internet. They use the Hypertext Transfer Protocol (HTTP) to send data to clients such as web browsers and receive requests from those clients. These are then rendered in the form Hypertext Markup Language (HTML) in the client's browser. This type of communication allows the client to create dynamic web pages that respond to the client's requests. Therefore, it is important that we understand the various functions of the web server in order to create secure and efficient web applications and also ensure the security of the system. Some of the most popular web servers for Linux servers are Apache, Nginx, Lighttpd, and Caddy. Apache is one of the most popular and widely used web servers and is available on a variety of operating systems, including Ubuntu, Solaris, and Redhat Linux.

As penetration testers, we can use web servers for a variety of purposes. For example, we can use a web server to perform file transfers, allowing us to log in and interact with a target system through an incoming HTTP or HTTPS port. Finally, we can use a web server to perform phishing attacks by hosting a copy of the target page on our own server and then attempting to steal user credentials. In addition, there is a variety of other possibilities.

Apache web server has a variety of features that allow us to host a secure and efficient web application. Moreover, we can also configure logging to get information about the traffic on our server, which helps us analyze attacks. We can install Apache using the following command:

### Install Apache Web Server

```Shell
bobinsson@BoB$ sudo apt install apache2 -y
```

For Apache2, to specify which folders can be accessed, we can edit the file <span style="color: #2dc26b;">/etc/apache/apache2.conf</span> with text editor. This file contains the global settings. We can change the settings to specify which directories can be accessed and what actions can be performed on those directories.

### Apache Configuration

```txt
<Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</directory>
```

This section specifies that the default <span style="color: #2dc26b;">/var/www/html</span> folder is accessible, that users can use the <span style="color: #2dc26b;">Indexes</span> and <span style="color: #2dc26b;">FollowSymLinks</span> options, that changes to files in this directory can be overridden with <span style="color: #2dc26b;">AllowOverride All</span>, and that <span style="color: #2dc26b;">Require all granted</span> grants all users access to this directory. For example, if we want to transfer files to one of our target systems using web server, we can put the appropriate files in the <span style="color: #2dc26b;">/var/www/html</span> folder and use <span style="color: #2dc26b;">wget</span> or <span style="color: #2dc26b;">curl</span> or other applications to download these files on the target system.

It is also possible to customize individual settings at the directory level by using the <span style="color: #2dc26b;">.htaccess</span> file, which we can create in the directory in question. This file allows us to configure certain directory-level settings, such as access controls, without having to customize the Apache configuration file. We can also add modules to get features like <span style="color: #2dc26b;">mod_rewrite, mod_security</span>, and <span style="color: #2dc26b;">mod_ssl</span> that help us improve the security of our web application.

Python Web Server is a simple, fast alternative to Apache and can be used to host a single folder with a single command to transfer files to another system. To install Python Web Server it's necessary to run the following command, with Python3 installed in the system:

### Install Python & Web Server

```Shell
bobinsson@BoB$ sudo apt install python3 -y
bobinsson@BoB$ python3 -m http.server
```

When running this command, the Python Web Server will be started on the <span style="color: #2dc26b;">TCP/8000</span> port, and it will be possible to access the folder the shell is currently in. Another folder can be hosted with the following command:

```Shell
bobinsson@BoB$ python3 -m http.server --directory /home/bobinsson/target_files
```

This will start a Python web server on the <span style="color: #2dc26b;">TCP/8000</span> port, and the <span style="color: #2dc26b;">/home/bobinsson/target_files</span> folder will be accessible through the browser, for example. When accessing the Python server, it's possible to transfer files to the other system by typing the link in the browser and downloading the files. It's also viable to host the Python web server on a port other than the default one:

```Shell
bobinsson@BoB$ python3 -m http.server 443
```

This will host the Python web server on port 443 instead of the default <span style="color: #2dc26b;">TCP/8000</span> port,, allowing access to the server by typing the link in the browser.

&nbsp;

## VPN

Virtual Private Network (<span style="color: #2dc26b;">VPN</span>) is a technology that allows secure connection to another network as if inside it. This is done by creating an encrypted tunnel connection between the client and the server, which means that all data transmitted over this connection is encrypted.

VPNs are mainly used by companies to provide employees with access to internal network without having to be physically located ate the corporate network. This allows employees to access the internal network and it's resources and applications from any location. In addition, VPNs can also be used to anonymize traffic and prevent outside access.

Some of the most popular VPN servers for Linux are OpenVPN, L2TP/IPsec, PPTP, SSTP, and SoftEther. OpenVPN is a popular open-source VPN server available for various operating systems, including Ubuntu, Solaris, and Redhat Linux. OpenVPN is used by administrators for various purposes, including enabling secure remote access to the corporate network, encrypting network traffic, and anonymizing traffic.

OpenVPN can also be used by penetration testers to connect to internal netwoorks. It can happen that a VPN access is created by the user so that the tester can test tjhe internal network vulnerabilities. This is an alternative for cases when the penetration tester is too far away from the customer. OpenVPN provides a variety of features, including encryption, tunneling, traffic shaping, network routing, and the ability to adapt to dynamically changing networks. The server and client can be installed using the following command:

### Install OpenVPN

```Shell
bobinsson@BoB$ sudo apt install openvpn -y
```

OpenVPN can be customized and configured by editing the configuration file <span style="color: #2dc26b;">/etc/openvpn/server.conf</span>. This file contains the settings for the OpenVPN server. The settings can be changed to configure certain features such as encryption, tunneling, traffic shaping, etc.

To connect to an OpenVPN server, the <span style="color: #2dc26b;">.ovpn</span> file received from the server can be used and saved on the system, by using the following command:

### Connect to VPN

```Shell
bobinsson@BoB$ sudo openvpn --config internal.ovpn
```

After the connection is established, it'll be possible to communicate with the internal hosts on the internal network.