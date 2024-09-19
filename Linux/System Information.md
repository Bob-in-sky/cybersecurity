# System Information

Commands that showcase basic information about the system:

- whoami - display current user
- id - returns user identity, the number which it's identified by in the system
    - expands on the whoami command and prints out effective group membership and IDs. With this penetration testers can see what access a user may have, and sysadmins can audit account permissions and group membership. the adm group means that the user can read log files in /var/log and could potentially gain access to sensitive information, membership in the sudo group is of particular interest as this means our user can run some or all commands as the all-powerful root user. Sudo rights could help us escalate privileges or could be a sign to a sysadmin that they may need to audit permissions and group memberships to remove any access that is not required for a given user to carry out their day-to-day tasks.
- hostname - sets or prints the name of current host system, the name of the computer in the network (ex: bobinsson@BoB, BoB is the name of the computer)

```Shell
bobinsson@BoB$ hostname
BoB
```

- uname - prints basic information about the operating system name and system hardware
- pwd - returns working directory name
- ifconfig - this utility is used to assign or to view an address to a network interface aand/or configure network interface parameters
- ip - show or manipulate routing, network devices, interfaces and tunnels
- netstat - shows network status, showcasing active internet connections
- ss - another utility to investigate sockets
- ps - shows process status
- who - shows who's logged in
- env - prints environment or sets and executes commands
- lsblk - lists block devices
- lsusb - lists USB devices
- lsof - lists open files
- lspci - lists PCI devices

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Questions

- <span style="color: #ffffff;">Find out the machine hardware name and submit it as the answer.</span>

&nbsp;

- <span style="color: #ffffff;"><span style="color: #ffffff;">What is the path to the user home directory?</span></span>

&nbsp;

- <span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;">What is the path to the user mail?</span></span></span>

&nbsp;

- <span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;">Which shell is specified for the user?</span></span></span></span>

&nbsp;

- <span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;">Which kernel version is installed on the system? (Format: 1.22.3)</span></span></span></span></span>

&nbsp;

- <span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;"><span style="color: #ffffff;">What is the name of the network interface that MTU is set to 1500?</span></span></span></span></span></span>