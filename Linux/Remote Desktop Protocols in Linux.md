# Remote Desktop Protocols in Linux

Remote desktop protocols are used in Windows, Linux, and MacOS to provide graphical remote access to a system. The administrators can utilize remote desktop protocols in many scenarios like troubleshooting, software or system upgrading, and remote systems administration. The administrator needs to connect to the remote system they will administer remotely, and therefore, they use the appropriate protocols if they want to install an application on their remote system. The most common protocols for this usage are RDP (Windows) and VNC (Linux).

&nbsp;

## XServer

The XServer is the user-side part of the <span style="color: #2dc26b;">X Window System network protocol (X11/ X)</span>. The <span style="color: #2dc26b;">X11</span> is a fixed system that consists of  collection of protocols and applications that allow us to call application windows on displays in a graphical user interface. <span style="color: #2dc26b;">X11</span> is predominant on Unix systems, but X servers are also available for other operating systems. Nowadays, the XServer is a part of almost every desktop installation of Ubuntu and its derivatives and does not need to be installed separately.

When a desktop is started on a Linux computer, the communication of the graphical user interface with the operating system happens via an X server. The computer's internal network is used, even if the computer should not be in a network. The practical thing about the X protocol is network transparency. This protocol mainly uses TCP/IP as a transport base but can also be used on pure Unix sockets. The ports that are utilized for X server are typically located in the range of <span style="color: #2dc26b;">TCP/6001-6009</span>, allowing communication between the client and server. When starting a new desktop session via X server the <span style="color: #2dc26b;">TCP port 6000</span> would be opened for the first X display <span style="color: #2dc26b;">:0</span>. This range of ports enables the server to perform its tasks such as hosting applications, as well as providing services services to clients. They are often used to provide remote access to a system, allowing users to access applications and data from anywhere in the world> additionally, these ports are also essential for the secure sharing of files and data, making them an integral prat of the Open X Server. Thus an X server is not dependent on the local computer, it can be used to access other computers, and other computers can use the local X server. Provided that both local and remote computers contains Unix/Linux systems, additional protocols such as VNC and RDP are superfluous. VNC and RDP generate the graphical output on the remote computer and transport it over the network. Whereas with X11, it is rendered on the local computer. This saves traffic and a load on the remote computer. However, X11's significant disadvantage is the unencrypted data transmission. However, this can be overcome by tunneling the SSH protocol.

For this, we have to allow X11 forwarding in the SSH configuration file (<span style="color: #2dc26b;">/etc/ssh/ssh_config</span>) on the server that provides the application by changing the option to <span style="color: #2dc26b;">yes</span>.

### X11 forwarding

```Shell
bobinsson@BoB$ cat /etc/ssh/sshd_config | grep X11Forwarding

X11 forwarding yes
```

With this we can start the application from out client with the following command:

```Shell
bobinsson@BoB$ ssh -X <user>@<target_host_ip> /usr/bin/firefox

<user>@<target_host_ip>'s password: ********
<SKIP>
```

This will open the interface of firefox in the client computer, but as executing in the target's machine.

### X11 Security

X11 is not a secure protocol without suitable security measures since X11 communication is entirely unencrypted. A completely open X server lets anyone on the network read the contents of its windows, for example, and this goes unnoticed by the user sitting in front of it. Therefore, it is not even necessary to sniff the network. This standard X11 functionality is realized with simple X11 tools like <span style="color: #2dc26b;">xwd</span> and <span style="color: #2dc26b;">xgrabsc</span>. In short, as penetration testers, we could read user's keystrokes, obtain screenshots, move the mouse cursor and send keystrokes from the server over the network.

A good example is several security vulnerabilities found in XServer, where a local attacker can exploit vulnerabilities in XServer to execute arbitrary code with user privilieges and gain user privileges. The operating systems affected by these vulnerabilities were Unix and Linux, Red Hat Enterprise Linux, Ubuntu Linux, and SUSE Linux. These vulnerabilities are known as CVE-2017-2624, CVE 2017-2625, and CVE 2017-2626.

&nbsp;

## XDMCP

The <span style="color: #2dc26b;">X Display Manager Control Protocol</span> (<span style="color: #2dc26b;">XDMCP</span>) protocol is used by the <span style="color: #2dc26b;">X Display Manager</span> for communication through UDP port 177 between X terminals and computers operating under Unix/Linux. It is used to manage remote X Window sessions on other machines and is often used by Linux system administrators to provide access to remote desktops. XDMCP is an insecure protocol and should not be used in any environment that requires high levels of security. With this, it is possible to redirect an entire graphical user interface (<span style="color: #2dc26b;">GUI</span>) (such as KDE or Gnome) to a corresponding client. For a Linux system to act as an XDMCP server, an X server with a GUI must be installed and configured on the server. After starting the computer, a graphical interface should be available locally to the user.

One potential way that XDMCP could be exploited is through a man-in the middle attack. In this type of attack, an attacker intercepts the communication between the remote computer and the X Window System server, and impersonates one of the parties in order to gain unauthorized access to the server. The attacker could then use the server to run arbitrary commands, access sensitive data, or perform other actions that could compromise the security of the system.

&nbsp;

## VNC

<span style="color: #2dc26b;">Virtual Network Computing</span> (<span style="color: #2dc26b;">VNC</span>) is a remote desktop sharing system based on the RFB protocol that allows users to control a computer remotely. It allows a user to view and interact with a desktop environment remotely over a network connection. The user can control the remote computer as if sitting in front of it. This is also one of the most common protocols for remote graphical connections for Linux hosts.

VNC is generally considered to be secure. It uses encryption to ensure the data is safe while in transit and requires authentication before a user can gain access. Administrators make use of VNC to access computers that are not physically accessible. This could be used to troubleshoot and maintain servers, access applications on other computers, or provide remote access to workstations. VNC can also be used for screen sharing, allowing multiple users to collaborate on a project or troubleshoot a problem.

There are two different concepts for VNC servers. The usual server offers the actual screen of the host computer for user support. Because the keyboard and mouse remain usable at the remote computer, an arrangement is recommended. The second group of server programs allows user login to virtual sessions, similar to the terminal server concept.

Server and viewer programs for VNC are available for all common operating systems. Therefore, many IT services are performed with VNC. The proprietary TeamViewer, and RDP have similar uses.

Traditionally, the VNC server listens on TCP port 5900. So it offers its <span style="color: #2dc26b;">display 0</span> there. Other displays can be offered via additional ports, mostly <span style="color: #2dc26b;">590\[x\]</span>, where <span style="color: #2dc26b;">x</span> is the display number. Adding multiple connections would assign them to a higher TCP port like 5901, 5902, 5903, etc.

For these VNC connections, many different tools are used. Among them are for example:

- [TigerVNC](https://tigervnc.org/)

- [TightVNC](https://www.tightvnc.com/)

- [RealVNC](https://www.realvnc.com/en/)

- [UltraVNC](https://uvnc.com/)

The most used tools for such kinds of connections are UltraVNC and RealVNC because of their encryption and higher security.

&nbsp;

### TigerVNC

In this example we setup a <span style="color: #2dc26b;">TigerVNC</span> server, and for this, we need, among other things, also the <span style="color: #2dc26b;">XFCE4</span> desktop manager since the VNC connections with GNOME are somewhat unstable. Therefore we need to install the necessary packages and create a password for the VNC connection.

These configuration steps are all executed in the target machine that we want to host the server to be accessed remotely.

### Installation

```Shell
bobinsson@BoB$ sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
bobinsson@BoB$ vncpasswd 

Password: ******
Verify: ******
Would you like to enter a view-only password (y/n)? n
```

### configuration

During installation a hidden folder is created in the home directory called <span style="color: #2dc26b;">.vnc</span>. Then, we have to create two additional files, <span style="color: #2dc26b;">xstartup</span> and <span style="color: #2dc26b;">config</span>. The xstartup determines how the VNC session is created in connection with the display manager, and the config determines its settings.

```Shell
bobinsson@BoB$ touch ~/.vnc/xstartup ~/.vnc/config
bobinsson@BoB$ cat << EOT >> ~/.vnc/xstartup

#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
EOT
```

```Shell
bobinsson@BoB$ cat <<EOT >> ~/.vnc/config

geometry=1920x1080
dpi=96
EOT
```

Additionally, the xstartup executable needs rights to be started by the service.

```Shell
bobinsson@BoB$ chmod +x ~/.vnc/xstartup
```

### Start the VNC Server

Now we can start the VNC server.

```Shell
bobinsson@BoB$ vncserver

New 'linux:1 (bobinsson)' desktop at :1 on machine linux

Starting applications specified in /home/bobinsson/.vnc/xstartup
Log file is /home/bobinsson/.vnc/linux:1.log

Use xtigervncviewer -SecurityTypes VncAuth -passwd /home/bobinsson/.vnc/passwd :1 to connect to the VNC server.
```

### List Sessions

In addition, we can also display the entire sessions with the associated ports and the process ID.

```Shell
bobinsson@BoB$ vncserver -list

TigerVNC server sessions:

X DISPLAY #     RFB PORT #      PROCESS ID
:1              5901            79746
```

### Setting Up an SSH Tunnel

To encrypt the connection and make it more secure, we can create as SSH tunnel over which the whole connection is tunneled. How tunneling works in detail can be learned at [Pivoting, Tunneling, and Port Forwarding](https://academy.hackthebox.com/module/details/158) .

```Shell
bobinsson@BoB$ ssh -L 5901:127.0.0.1:5901 -N -f -l <target_user> <target_host_ip>

<target_user>@<target_host_ip>'s password: *******
```

### Connecting to the VNC Server

Finally, we can connect to the server through the SSH tunnel using the <span style="color: #2dc26b;">xtightvncviewer</span>.

```Shell
bobinsson@BoB$ xtightvncviewer localhost:5901

Connected to RFB server, using protocol version 3.8
Performing standard VNC authentication

Password: ******

Authentication successful
Desktop name "linux:1 (<target>)"
VNC server default format:
  32 bits per pixel.
  Least significant byte first in each pixel.
  True colour: max red 255 green 255 blue 255, shift red 16 green 8 blue 0
Using default colormap which is TrueColor.  Pixel format:
  32 bits per pixel.
  Least significant byte first in each pixel.
  True colour: max red 255 green 255 blue 255, shift red 16 green 8 blue 0
Same machine: preferring raw encoding
```