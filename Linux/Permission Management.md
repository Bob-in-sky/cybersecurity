---
title: Permission Management
updated: 2024-09-26 21:00:44Z
created: 2024-09-20 01:48:16Z
---

# Permission Management

Under Linux, permissions are assigned to users and groups. Each user can be a member of different groups, and membership in these groups gives the user specific, additional permissions. Each file and directory belongs to a specific user and a specific group. So the permissions for users and groups that defined a file are also defined for the respective owners. When we create new files or directories, they belong to the group we belong to, and to us.

When a user wants to access the contents of a Linux directory, they must first traverse the directory, which means navigating to that directory, requiring the user to have <span style="color: #2dc26b;">execute</span> permissions on the directory. Without this permission, the user cannot access the directory's contents and will instead be presented with a "<span style="color: #2dc26b;">Permission denied</span>" error message.

```Shell
bobinsson@BoB$ ls -l /root/
ls: cannot open directory '/root/': Permission denied
```

It is important to note that <span style="color: #2dc26b;">execute</span> permissions are necessary to traverse a directory, no matter the user's level of access. Also, <span style="color: #2dc26b;">execute</span> permissions on a directory do not allow a user to execute or modify any of the files or contents within the directory, only to traverse and access the content of the directory.

To execute files within the directory, a user needs <span style="color: #2dc26b;">execute</span> permissions on the corresponding file. To modify the contents of a directory (create, delete, or rename files and subdirectories), the user needs <span style="color: #2dc26b;">write</span> permissions on the directory.

The whole permission system on Linux systems is based on the octal number system, and basically, there are three different types of permissions a file can be assigned:

- ( <span style="color: #2dc26b;">r</span> ) - Read
- ( <span style="color: #2dc26b;">w</span> ) - Write
- ( <span style="color: #2dc26b;">x</span> ) - Execute

The permissions can be set for the <span style="color: #2dc26b;">owner</span>, <span style="color: #2dc26b;">group</span>, and <span style="color: #2dc26b;">others</span> like presented in the next example with their corresponding permissions.

```Shell
bobinsson@BoB$ ls -l /etc/passwd

- rwx rw- r--   1 root root 1641 May  4 23:42 /etc/passwd
- --- --- ---   |  |    |    |   |__________|
|  |   |   |    |  |    |    |        |_ Date
|  |   |   |    |  |    |    |__________ File Size
|  |   |   |    |  |    |_______________ Group
|  |   |   |    |  |____________________ User
|  |   |   |    |_______________________ Number of hard links
|  |   |   |_ Permission of others (read)
|  |   |_____ Permissions of the group (read, write)
|  |_________ Permissions of the owner (read, write, execute)
|____________ File type (- = File, d = Directory, l = Link, ... )
```

&nbsp;

## Change Permission

We can modify permissions using the <span style="color: #2dc26b;">chmod</span> command, permission group references (<span style="color: #2dc26b;">u</span> - owner, <span style="color: #2dc26b;">g</span> - group, <span style="color: #2dc26b;">o</span> - others, <span style="color: #2dc26b;">a</span> - all users), and either a \[ <span style="color: #2dc26b;">+</span> \] or a \[ <span style="color: #2dc26b;">\-</span> \] to add or remove the designated permissions. Inn the following example, let us assume we have a file called shell and we want to change permissions for it so this script is owned by the user, becomes not executable and set with read/write permissions for all users.

```Shell
bobinsson@BoB$ ls -l shell
-rwxr-x--x   1 bobinsson bobgroup 0 May  4 22:12 shell
```

We can then apply <span style="color: #2dc26b;">read</span> permissions for all users and see the result.

```Shell
bobinsson@BoB$ chmod a+r shell && ls -l shell
-rwxr-xr-x   1 bobinsson bobgroup 0 May  4 22:12 shell
```

We can also set the permissions for all other users to <span style="color: #2dc26b;">read</span> only using the octal value assignment.

```Shell
bobinsson@BoB$ chmod 754 shell && ls -l shell
-rwxr-xr--   1 bobinsson bobgroup 0 May  4 22:12 shell
```

Let us look at all the representations associated with it to understand better how the permission assignment is calculated.

```Shell
Binary Notation:                4 2 1  |  4 2 1  |  4 2 1
----------------------------------------------------------
Binary Representation:          1 1 1  |  1 0 1  |  1 0 0
----------------------------------------------------------
Octal Value:                      7    |    5    |    4
----------------------------------------------------------
Permission Representation:      r w x  |  r - x  |  r - -
```

If we sum the set bits from the <span style="color: #2dc26b;">Binary Representation</span> assigned to the values from <span style="color: #2dc26b;">Binary Notation</span> together we get the <span style="color: #2dc26b;">Octal Value</span>. The <span style="color: #2dc26b;">Permission Representation</span> represents the bits set in the <span style="color: #2dc26b;">Binary Representation</span> by using the three characters, which only recognizes the set permissions easier.

&nbsp;

## Change Owner

To change the owner and/or the group assignments of a file or directory, we can use the <span style="color: #2dc26b;">chown</span> command. The syntax is like following:

### chown

```Shell
bobinsson@BoB$ chown <user>:<group> <file/directory>
```

In this example, "shell" can be replaced with any arcitrary folder.

```Shell
bobinsson@BoB$ chown root:root shell && ls -l shell
-rwxr-xr--   1 root root 0 May  4 22:12 shell
```

&nbsp;

## SUID & SGID

Besides assigning direct user and group permissions, we can also configure special permissions for files by setting the <span style="color: #2dc26b;">Set User ID</span> (<span style="color: #2dc26b;">SUID</span>) and <span style="color: #2dc26b;">Set Group ID</span> (<span style="color: #2dc26b;">SGID</span>) bits. These <span style="color: #2dc26b;">SUID</span>/<span style="color: #2dc26b;">SGID</span> bits allow, for example, users to run programs with the rights of another user. Administrators often use this to give their users special rights for certain applications or files. The letter "<span style="color: #2dc26b;">s</span>" is used instead of an "<span style="color: #2dc26b;">x</span>". When executing such a program, the SUID/SGID of the file owner is used.

It is often the case that administrators are not familiar with the applications but still assign the SUID/SGID bits, which leads to a high-security risk. Such programs may contain functions that allow the execution of a shell from the pager, such as the application "<span style="color: #2dc26b;">journalctl</span>"

If the administrator sets the SUID bit to "<span style="color: #2dc26b;">journalctl</span>", any user with access to this application could execute a shell as <span style="color: #2dc26b;">root</span>. More information about this and other such applications can be found at https://gtfobins.github.io/gtfobins/journalctl/

&nbsp;

## Sticky Bit

Sticky bits are a type of file permission in Linux that can be set on directories. This type of permission provides an extra layer of security when controlling the deletion and renaming of files within a directory. It is typically used on directories that are shared by multiple users to prevent one user from accidentally deleting or renaming files that are important to others.

For example, in a shared home directory, where multiple users have access to the same directory, a system administrator can set the sticky bit on the directory to ensure that only the owner of the file, the owner of the directory, or the root user can delete or rename files within the directory. This means that other users cannot delete or rename files within the directory as they do not have the required permissions. This provides an added layer of security to protect important files, as only those with the necessary access can delete or rename files. Setting the sticky bit on a directory ensures that only the owner, the directory owner, or the root user can change the files within the directory.

When a sticky bit is set on a directory, it is represented by the letter "<span style="color: #2dc26b;">t</span>" in the execute permission of the directory's permissions. For example, if a directory has permissions “<span style="color: #2dc26b;">rwxrwxrwt</span>", it means that the sticky bit is set, giving the extra level of security so that no one other than the owner or the root user can delete or rename the files or folders in the directory.

```Shell
bobinsson@BoB$ ls -l

drw-rw-r-t 3 bobinsson bobinsson   4096 Jan 12 12:30 scripts
drw-rw-r-T 3 bobinsson bobinsson   4096 Jan 12 12:32 reports
```

In this example, we see that both directories have the sticky bit set. However, the <span style="color: #2dc26b;">reports</span> folder has an uppercase <span style="color: #2dc26b;">T</span>, and the <span style="color: #2dc26b;">scripts</span> folder has a lowercase t.

If the sticky bit is capitalized ( <span style="color: #2dc26b;">T</span> ), then this means that all other users do not have <span style="color: #2dc26b;">execute</span> ( <span style="color: #2dc26b;">x</span> ) permissions and, therefore, cannot see the contents of the folder nor run any programs from it. The lowercase sticky bit ( <span style="color: #2dc26b;">t</span> ) is the sticky bit where <span style="color: #2dc26b;">execute</span> ( <span style="color: #2dc26b;">x</span> ) permissions have been set.