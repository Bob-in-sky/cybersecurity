# User Management

User management is an essential part of Linux administration. Sometimes we need to create new users or add other users to specific groups. Another possibility is to execute commands as a different user. After all, it is not too rare that users of only one specific group have the permissions to view or edit specific files or directories. This, in turn, allows us to collect more information locally on the machine, which can be very important. Let us take a look at the following example of how to execute code as a different user.

&nbsp;

### Execution as a User

```Shell
bobinsson@BoB$ cat /etc/shadow

cat: /etc/shadow: Permission denied
```

&nbsp;

### Execution as root

```Shell
bobinsson@BoB$ sudo cat /etc/shadow

root:<SNIP>:18395:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
<SNIP>
```

&nbsp;

Here is a table that will help us to better understand and deal with user management

| Command | Description |
| --- | --- |
| <span style="color: #2dc26b;">sudo</span> | Execute command as a different user |
| <span style="color: #2dc26b;">su</span> | The <span style="color: #2dc26b;">su</span> utility requests appropriate user credentials via PAM and switches to that user ID (the default user is the superuser). A shell is then executed |
| <span style="color: #2dc26b;">useradd</span> | Creates a new user or update default new user information |
| <span style="color: #2dc26b;">userdel</span> | Deletes a user account and related files |
| <span style="color: #2dc26b;">usermod</span> | Modifies a user account |
| <span style="color: #2dc26b;">addgroup</span> | Adds a group for the system |
| <span style="color: #2dc26b;">delgroup</span> | Removes a group from the system |
| <span style="color: #2dc26b;">passwd</span> | Changes user password |

&nbsp;

User management is essential in any operating system, and the best way to become familiar with it is to try out the individual commands in conjunction with their various option.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Questions

- Which option needs to be set to create a home directory for a new user using "useradd" command?

\-m

- Which option needs to be set to lock a user account using the "usermod" command? (long version of the option)

\--lock

- Which option needs to be set to execute a command as a different user using the "su" command? (long version of the option)

\--command