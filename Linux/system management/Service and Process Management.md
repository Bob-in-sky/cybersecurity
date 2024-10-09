# Service and Process Management

In general, there are two types of services: internal, the relevant services that are required at system startup, which for example perform hardware-related tasks. And services that are installed by the user, which usually include all server services. Such services run in the background without any user interaction. These are also called <span style="color: #2dc26b;">daemons</span> and are identified by the letter "<span style="color: #2dc26b;">d</span>" at the end of the program name, for example <span style="color: #2dc26b;">sshd</span> or <span style="color: #2dc26b;">systemd</span>.

Most Linux distros have now switched to <span style="color: #2dc26b;">systemd</span>. This daemon is an <span style="color: #2dc26b;">Init process</span> started first and thus has the process ID (PID) 1. This daemon monitors and takes care of the orderly starting and stopping of other services. All processes have an assigned PID that can be viewed under <span style="color: #2dc26b;">/proc/</span> with the corresponding number. Such a process can have a parent process ID (PPID), and if so, it is known as the child process.

Besides <span style="color: #2dc26b;">systemctl</span> we can also use <span style="color: #2dc26b;">update-rc.d</span> to manage SysV init script links. Let us have a look at some examples. We will use the <span style="color: #2dc26b;">OpenSSH</span> server in these examples. If we do not have this installed, please install it before proceeding to this section.

&nbsp;

## Systemctl

After installing <span style="color: #2dc26b;">OpenSSH</span> on our VM, we can start the service with the following command.

```Shell
bobinsson@BoB$ systemctl start ssh
```

After we have started the service, we can now check if it runs without errors.

```Shell
bobinsson@BoB$ systemctl status ssh

● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-05-14 15:08:23 CEST; 24h ago
   Main PID: 846 (sshd)
   Tasks: 1 (limit: 4681)
   CGroup: /system.slice/ssh.service
           └─846 /usr/sbin/sshd -D

Mai 14 15:08:22 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 14 15:08:23 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:23 inlane sshd[846]: Server listening on :: port 22.
Mai 14 15:08:23 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 14 15:08:30 inlane systemd[1]: Reloading OpenBSD Secure Shell server.
Mai 14 15:08:31 inlane sshd[846]: Received SIGHUP; restarting.
Mai 14 15:08:31 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:31 inlane sshd[846]: Server listening on :: port 22.
```

To add OpenSSH to the SysV script to tell the system to run this service after startup, we can link it with the following command:

```Shell
bobinsson@BoB$ systemctl enable ssh

Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
```

Once we reboot the system, the OpenSSH server will automatically run. We can check this with a tool called <span style="color: #2dc26b;">ps</span>.

```Shell
bobinsson@BoB$ ps -aux | grep ssh

root       846  0.0  0.1  72300  5660 ?        Ss   Mai14   0:00 /usr/sbin/sshd -D
```

We can also use <span style="color: #2dc26b;">systemctl</span> to list all services

```Shell
bobinsson@BoB$ systemctl list-units --type=service

UNIT                                                       LOAD   ACTIVE SUB     DESCRIPTION              
accounts-daemon.service                                    loaded active running Accounts Service         
acpid.service                                              loaded active running ACPI event daemon        
apache2.service                                            loaded active running The Apache HTTP Server   
apparmor.service                                           loaded active exited  AppArmor initialization  
apport.service                                             loaded active exited  LSB: automatic crash repor
avahi-daemon.service                                       loaded active running Avahi mDNS/DNS-SD Stack  
bolt.service                                               loaded active running Thunderbolt system service
```

It is quite possible that the services do not start due to an error. To see the problem, we can use the tool <span style="color: #2dc26b;">journalctl</span> to view the logs

```Shell
bobinsson@BoB$ journalctl -u ssh.service --no-pager

-- Logs begin at Wed 2020-05-13 17:30:52 CEST, end at Fri 2020-05-15 16:00:14 CEST. --
Mai 13 20:38:44 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 13 20:38:44 inlane sshd[2722]: Server listening on 0.0.0.0 port 22.
Mai 13 20:38:44 inlane sshd[2722]: Server listening on :: port 22.
Mai 13 20:38:44 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 13 20:39:06 inlane sshd[3939]: Connection closed by 10.22.2.1 port 36444 [preauth]
Mai 13 20:39:27 inlane sshd[3942]: Accepted password for master from 10.22.2.1 port 36452 ssh2
Mai 13 20:39:27 inlane sshd[3942]: pam_unix(sshd:session): session opened for user master by (uid=0)
Mai 13 20:39:28 inlane sshd[3942]: pam_unix(sshd:session): session closed for user master
Mai 14 02:04:49 inlane sshd[2722]: Received signal 15; terminating.
Mai 14 02:04:49 inlane systemd[1]: Stopping OpenBSD Secure Shell server...
Mai 14 02:04:49 inlane systemd[1]: Stopped OpenBSD Secure Shell server.
-- Reboot --
```

&nbsp;

## Kill a Process

A process can be in the following states:

- Running
- Waiting (waiting for an event or system resource)
- Stopped
- Zombie (stopped but still has an entry in the process table)

Processes can be controlled using <span style="color: #2dc26b;">kill</span>, <span style="color: #2dc26b;">pkill</span>, <span style="color: #2dc26b;">pgrep</span>, and <span style="color: #2dc26b;">killall</span>. To interact with a process, we must send a signal to it. We can view all signals with the following command:

```Shell
bobinsson@BoB$ kill -l

 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```

&nbsp;

The most commonly used are:

| Signal | Description |
| --- | --- |
| <span style="color: #2dc26b;">1 - SIGHUP</span> | Sent to a process when the terminal that controls it is closed |
| <span style="color: #2dc26b;">2 - SIGINT</span> | Sent when a user presses <span style="color: #2dc26b;">\[ CTRL \] + C</span> in the controlling terminal to interrupt a process |
| <span style="color: #2dc26b;">3 - SIGQUIT</span> | Sent when a user presses <span style="color: #2dc26b;">\[ CTRL \] + D</span> to quit |
| <span style="color: #2dc26b;">9 - SIGKILL</span> | Immediately kill a process with no clean-up operations |
| <span style="color: #2dc26b;">15 - SIGTERM</span> | Program termination |
| <span style="color: #2dc26b;">19 - SIGSTOP</span> | Stop the program. It cannot be handled anymore |
| <span style="color: #2dc26b;">20 - SIGTSTP</span> | Sent when a user presses <span style="color: #2dc26b;">\[ CTRL \] + Z</span> to request for a service to suspend. The user can handle it afterwards |

&nbsp;

For example, if a program were to freeze, we could force to kill it with the following command:

```Shell
bobinsson@BoB$ kill 9 <PID> 
```

&nbsp;

## Background a Process

Sometimes it will be necessary to put the scan or process we just started in the background to continue using the current session to interact with the system or start other processes. As we have already seen, we can do this with the shortcut <span style="color: #2dc26b;">\[ CTRL + Z \]</span>. As mentioned above, we send the <span style="color: #2dc26b;">SIGTSTP</span> signal to the kernel, which suspends the process.

```Shell
bobinsson@BoB$ ping -c 10 www.hackthebox.eu

bobinsson@BoB$ vim tmpfile
[Ctrl + Z]
[2]+  Stopped                 vim tmpfile
```

Now all background processes can be displayed with the following command.

```Shell
bobinsson@BoB$ jobs

[1]+  Stopped                 ping -c 10 www.hackthebox.eu
[2]+  Stopped                 vim tmpfile
```

The <span style="color: #2dc26b;">\[ CTRL + Z \]</span> shortcut suspends the processes, and they will not be executed further. To keep it running in the background, we have to enter the command <span style="color: #2dc26b;">bg</span> to put the process in the background.

```Shell
bobinsson@BoB$ bg

bobinsson@BoB$ 
--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 16066ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
```

Another option is to automatically set the process with an AND sign ( <span style="color: #2dc26b;">&</span> ) at the end of the command.

```Shell
bobinsson@BoB$ ping -c 10 www.hackthebox.eu &

[1] 10825
PING www.hackthebox.eu (172.67.1.1) 56(84) bytes of data.
```

Once the process finishes, we will see the results.

```Shell
bobinsson@BoB$ 

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9210ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
```

&nbsp;

## Foreground a Process

After that, we can use the <span style="color: #2dc26b;">jobs</span> command to list all background processes. Background processes do not require user interaction, and we can use the same shell session without waiting until the process finishes first. Once the scan or process finishes its work, we will get notified by the terminal that the process is finished.

```Shell
bobinsson@BoB$ jobs

[1]+  Running                 ping -c 10 www.hackthebox.eu &
```

If we want to get the background process into the foreground and interact with it again, we can use the <span style="color: #2dc26b;">fg &lt;ID&gt;</span> command.

```Shell
bobinsson@BoB$ fg 1
ping -c 10 www.hackthebox.eu

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9206ms
```

&nbsp;

## Execute Multiple Commands

There are three possibilities to run several commands, one after the other. These are separated by:

- Semicolon ( <span style="color: #2dc26b;">;</span> )
- Double <span style="color: #2dc26b;">ampersand</span> characters ( <span style="color: #2dc26b;">&&</span> )
- Pipes ( <span style="color: #2dc26b;">|</span> )

The difference between them lies in the previous processes' treatment and depends on whether the previous process was completed successfully or with errors. The semicolon ( <span style="color: #2dc26b;">;</span> ) is a command separator and executes the commands by ignoring previous commands' results and errors.

```Shell
bobinsson@BoB$ echo '1'; echo '2'; echo '3'

1
2
3
```

For example, if we execute the same command but replace it in second place, the command <span style="color: #2dc26b;">ls</span> with a file that does not exist, we get an error, and the third command will be executed nevertheless.

```Shell
bobinsson@BoB$ echo '1'; ls MISSING_FILE; echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
3
```

&nbsp;

However, when using the double ampersand characters ( <span style="color: #2dc26b;">&&</span> ) to run the commands one after the other, following commands will not be executed at the occurrence of errors in one of the previous commands, and the whole process will be stopped.

```Shell
bobinsson@BoB$ echo '1' && ls MISSING_FILE && echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
```

&nbsp;

The pipes character ( <span style="color: #2dc26b;">|</span> ) depends not only on the correct and error-free operation of the previous processes, but also on the previous process results, since it provides the output of the previous command as input for the following command.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Questions

- Use the "systemctl" command to list all units of services and submit the unit name with the description "Load AppArmor profiles managed internally by snapd" as the answer.

snapd.apparmor.service