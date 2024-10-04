# File System Management

File system management on Linux is a complex process that involves organizing and maintaining the data stored on a disk or other storage device. Linux is a powerful operating system that supports a wide range of file systems, including ext2, ext3, ext4, XFS, and more. Each of these file systems offers unique features and benefits, and the best choice for any given situation will depend upon the specific requirements of the application or user. For example, ext2 is suitable for basic file system management tasks, while Btrfs offers robust data integrity and snapshot capabilities. Additionally, NTFS is useful when compatibility with Windows is required. No matter the situation, it is important to properly analyze the needs of the application or user before selecting a file system.

The Linux file system is based on the Unix file system. which is a hierarchical structure that is composed of various components. At the top of this structure is the inode table, the basis for the entire file system. The inode table is a table of information associated with each file and directory on a Linux system. Inodes contain metadata about the file or directory, such as its permissions, size, type, owner, and so on. The inode table is  like a database of information about every file and directory on a Linux system, allowing the operating system to quickly access and manage files. Files can be stored in the Linux file system in one of two ways:

- Regular files
- Directories

Regular files are the most common type of file, and they are stored in the root directory of the file system. Directories are used to store collections of files. When a file is stored in a directory, the directory is known as the parent directory for the files. In addition to regular files and directories, Linux also supports symbolic links, which are references to other files or directories. Symbolic links can be used to quickly access files that are located in different parts of the file system. Each file and directory needs to be managed in terms of permissions. Permissions control who has access to files and directories. Each file and directory has an associated set of permissions that determines who can read, write, and execute the file. The same permissions apply to all users, so if the permissions of one user are changed, all other users will also be affected.

```Shell
bobinsson@BoB$ ls -il

total 0
10678872 -rw-r--r--  1 bobinsson  bob  234123 Feb 14 19:30 myscript.py
10678869 -rw-r--r--  1 bobinsson  bob   43230 Feb 14 11:52 notes.txt
```

&nbsp;

## Disks & Drives

Disk management on Linux involves managing physical storage devices, including hard drives, solid-state drives, and removable storage devices. The main tool for disk management on Linux is the <span style="color: #2dc26b;">fdisk</span>, which allows to create, delete, and manage partitions on a drive. It can also display information about the partition table, including the size and type of each partition. Partitioning a drive on Linux involves dividing the physical storage space into separate, logical sections. Each partition can then be formatted with a specific file system, such as ext4, or FAT32, and can be mounted as a separate file system. The most common partitioning tool on Linux is also <span style="color: #2dc26b;">fdisk</span>, <span style="color: #2dc26b;">gpart</span>, and <span style="color: #2dc26b;">GParted</span>.

### fdisk

```Shell
bobinsson@BoB$ sudo fdisk -l

Disk /dev/vda: 160 GiB, 171798691840 bytes, 335544320 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x5223435f

Device     Boot     Start       End   Sectors  Size Id Type
/dev/vda1  *         2048 158974027 158971980 75.8G 83 Linux
/dev/vda2       158974028 167766794   8792767  4.2G 82 Linux swap / Solaris

Disk /dev/vdb: 452 KiB, 462848 bytes, 904 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

&nbsp;

## Mounting

Each logical partition or drive needs to be assigned to a specific directory on Linux. This process is called mounting. Mounting involves attaching a drive to a specific directory, making it accessible to the file system hierarchy. Once a drive is mounted, it can be accessed and manipulated just like any other directory on the system. The <span style="color: #2dc26b;">mount</span> tool is used to mount file systems on Linux, and the <span style="color: #2dc26b;">/etc/fstab</span> file is used to define the default file systems that are mounted at boot time.

### Mounted File Systems at Boot

```Shell
bobinsson@BoB$ cat /etc/fstab

# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a device; this may
# be used with UUID= as a more robust way to name devices that works even if
# disks are added and removed. See fstab(5).
#
# <file system>                      <mount point>  <type>  <options>  <dump>  <pass>
UUID=3d6a020d-...SNIP...-9e085e9c927a /              btrfs   subvol=@,defaults,noatime,nodiratime,nodatacow,space_cache,autodefrag 0 1
UUID=3d6a020d-...SNIP...-9e085e9c927a /home          btrfs   subvol=@home,defaults,noatime,nodiratime,nodatacow,space_cache,autodefrag 0 2
UUID=21f7eb94-...SNIP...-d4f58f94e141 swap           swap    defaults,noatime 0 0

```

### List Mounted Drives

To view the current mounted file systems, it's possible to use the "<span style="color: #2dc26b;">mount</span>" command without any arguments. The output will show a list of all the currently mounted file systems, including the device name, file system type, mount point, and options.

```Shell
bobinsson@BoB$ mount

sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=4035812k,nr_inodes=1008953,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=814580k,mode=755,inode64)
/dev/vda1 on / type btrfs (rw,noatime,nodiratime,nodatasum,nodatacow,space_cache,autodefrag,subvolid=257,subvol=/@)
```

### Mount a USB Drive

To mount a file system, it's possible to use the the <span style="color: #2dc26b;">mount</span> command followed by the device name and the mount point. For example, to mount a USB drive with the device name <span style="color: #2dc26b;">/dev/sdb1</span> to the directory <span style="color: #2dc26b;">/mnt/usb</span>, the following command can be used:

```Shell
bobinsson@BoB$ sudo mount /dev/sdb1 /mnt/usb
bobinsson@BoB$ cd /mnt/usb && ls -l

total 32
drwxr-xr-x 1 root root   18 Oct 14  2021 'Account Takeover'
drwxr-xr-x 1 root root   18 Oct 14  2021 'API Key Leaks'
drwxr-xr-x 1 root root   18 Oct 14  2021 'AWS Amazon Bucket S3'
drwxr-xr-x 1 root root   34 Oct 14  2021 'Command Injection'
drwxr-xr-x 1 root root   18 Oct 14  2021 'CORS Misconfiguration'
drwxr-xr-x 1 root root   52 Oct 14  2021 'CRLF Injection'
drwxr-xr-x 1 root root   30 Oct 14  2021 'CSRF Injection'
drwxr-xr-x 1 root root   18 Oct 14  2021 'CSV Injection'
drwxr-xr-x 1 root root 1166 Oct 14  2021 'CVE Exploits'
...SNIP...
```

### unmount

To unmount a file system in Linux, the command <span style="color: #2dc26b;">unmount</span> can be used followed by the mount point of the file system to be unmounted. The mount point is the location in the file system where the file system is mounted and is accessible. For example, to unmount the USB drive that was previously mounted to the directory <span style="color: #2dc26b;">/mnt/usb</span>, the following command can be used:

```Shell
bobinsson@BoB$ sudo unmount /mnt/usb
```

It is important to note that there must be sufficient permissions to unmount a file system. It is also not possible to unmount a system that is in use by a running process. To ensure that there are no running processes that are using the file system, the command <span style="color: #2dc26b;">lsof</span> can be used to list the open files on the file system.

```Shell
bobinsson@BoB$ lsof | grep bobinsson

vncserver 6006        bobinsson  mem       REG      0,24       402274 /usr/bin/perl (path dev=0,26)
vncserver 6006        bobinsson  mem       REG      0,24      1554101 /usr/lib/locale/aa_DJ.utf8/LC_COLLATE (path dev=0,26)
vncserver 6006        bobinsson  mem       REG      0,24       402326 /usr/lib/x86_64-linux-gnu/perl-base/auto/POSIX/POSIX.so (path dev=0,26)
vncserver 6006        bobinsson  mem       REG      0,24       402059 /usr/lib/x86_64-linux-gnu/perl/5.32.1/auto/Time/HiRes/HiRes.so (path dev=0,26)
vncserver 6006        bobinsson  mem       REG      0,24      1444250 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so (path dev=0,26)
vncserver 6006        bobinsson  mem       REG      0,24       402327 /usr/lib/x86_64-linux-gnu/perl-base/auto/Socket/Socket.so (path dev=0,26)
vncserver 6006        bobinsson  mem       REG      0,24       402324 /usr/lib/x86_64-linux-gnu/perl-base/auto/IO/IO.so (path dev=0,26)
...SNIP...
```

### fstab File

If there are any processes still running in the file system, they need to be stopped before it is permitted to unmount the file system. Additionally, it can also be unmounted automatically when the system is shut down by adding an entry to the <span style="color: #2dc26b;">/etc/fstab</span> file. The <span style="color: #2dc26b;">/etc/fstab</span> file contains information about all the file systems that are mounted on the system, including the options for automatic mounting at boot time and other mount options. To unmount a file system automatically at shutdown, it is needed to add the <span style="color: #2dc26b;">noauto</span> option to the entry in the <span style="color: #2dc26b;">/etc/fstab</span> file for that file system. This would look like, for example, the following:

```Shell
/dev/sda1 / ext4 defaults 0 0
/dev/sda2 /home ext4 defaults 0 0
/dev/sdb1 /mnt/usb ext4 rw,noauto,user 0 0
192.168.1.100:/nfs /mnt/nfs nfs defaults 0 0
```

&nbsp;

## SWAP

Swap space is a crucial aspect of memory management in Linux, and it plays an important role in ensuring that the system runs smoothly, even when the available physical memory is depleted. When the system runs out of physical memory, the kernel transfers inactive pages of storage to the swap space, freeing up physical memory for use by active processes. This process is known as swapping.

Swap space can be created either during the installation of the operating system or at any time afterward using the <span style="color: #2dc26b;">mkswap</span> and <span style="color: #2dc26b;">swapon</span> commands. The <span style="color: #2dc26b;">mkswap</span> command is used to set up a Linux swap area on a device or in a file, while the <span style="color: #2dc26b;">swapon</span> command is used to activate a swap area. The size of the swap area is a matter of personal preference and depends on the amount of physical storage and memory installed in the system and the type of usage the system will be subjected to. When creating a swap space, it is important to ensure that it is places on a dedicated partition or file, separate from the rest of the file system. This helps to prevent fragmentation of the swap space and ensures that the system has adequate swap space available when it is needed. It is also important to ensure that the swap space is encrypted, as sensitive data may be stored in the swap space temporarily.

In addition to being used as an extension of physical memory, swap space can also be used for hibernation, which is a power management feature that allows the system to save its state to disk and then power off instead of shutting down completely. When the system is later powered on, it can restore its state from the swap space, returning to the state it was in before it was powered off.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# Questions

- How many partitions exit in your machine?