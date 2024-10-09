# Backup and Restore

Linux systems offer a variety of software tools for backing up and restoring data. These tools are designed to be efficient and secure, ensuring that data is protected while also allowing to easily access it when needed.

Backing up data on Ubuntu systems can be made using some of the followings tools:

- Rsync
- Deja Dup
- Duplicity

Rsync is an open-source tool that allows to quickly and securely back up files and folders to a remote location. It is particularly useful for transferring large amounts of data over the network, as it only transmits the changed parts of a file. It can also be used to create backups locally or on remote servers. If it is needed to back up large amounts of data over the network, Rsync might be the better option.

Duplicity is another graphical back up tool for Ubuntu that provides the users with comprehensive data protection and secure backups. It also uses Rsync as a backend and additionally offers the possibility to encrypt backup copies and store them on remote storage media, such as FTP servers, or cloud storage services, such as amazon S3.

Deja Dup is a graphical backup tool for Ubuntu that simplifies the backup process, allowing to quickly and easily back up our data. It provides a user-friendly interface to create backup copies of data on local or remote storage media. It uses Rsync as a backend and also supports data encryption.

In order to ensure the security and integrity of backups, it's recommended to take steps towards encrypting the data. Encrypting backups ensures that sensitive data is protected from unauthorized access. That is possible to be implemented on Ubuntu systems by utilizing tools such as GnuPG, eCryptfs, and LUKS.

Backing up and restoring data on Ubuntu systems is an essential part of data protection. By utilizing the tools discussed, it is possible to ensure that the data is securely backed up and can be easily restored when needed.

&nbsp;

## Rsync

### Install Rsync

Rsync can be installed by using the <span style="color: #2dc26b;">apt</span> package manager:

```Shell
bobinsson@BoB$ sudo apt install rsync -y
```

This will install the latest version of Rsync on the system. Once the installation is complete, we can begin using the tool to back up and restore data. To backup an entire directory using <span style="color: #2dc26b;">rsync</span>, the following command can be used:

### Backup a local Directory to the Backup-Server

```Shell
bobinsson@BoB$ rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

This command will copy the entire directory (<span style="color: #2dc26b;">/path/to/mydirectory</span>) to the directory /path/to/backup/directory inside a remote host (<span style="color: #2dc26b;">backup_server</span>). The option <span style="color: #2dc26b;">archive</span> ( <span style="color: #2dc26b;">\-a</span> ) is used to preserve the original file attributes, such as permissions, timestamps, etc., and using the <span style="color: #2dc26b;">verbose</span> ( <span style="color: #2dc26b;">\-v</span> ) option provides a detailed output of the progress of the <span style="color: #2dc26b;">rsync</span> operation.

Additional options can be added to optimize the backup process, such as using compression and incremental backups, which can be accomplished by doing the following:

```Shell
bobinsson@BoB$ rsync -avz --backup --backup-dir=/path/to/backup/folder --delete /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

With this, <span style="color: #2dc26b;">mydirectory</span> is backed up to the remote <span style="color: #2dc26b;">backup_server</span>, preserving the original file attributes, timestamps, and permissions, and enabling compression ( <span style="color: #2dc26b;">\-z</span> ) for faster transfers. The <span style="color: #2dc26b;">\--backup</span> option creates incremental backups in the directory <span style="color: #2dc26b;">/path/to/backup/folder</span>, and the <span style="color: #2dc26b;">\--delete</span> option removes files from the remote host that are no longer present in the source directory.

### Restore the Backup

To restore the directory from the backup server to the local directory, the following command can be used:

```Shell
bobinsson@BoB$ rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory
```

&nbsp;

## Encrypted Rsync

To ensure the security of the <span style="color: #2dc26b;">rsync</span> file transfer between the local host and the backup server, it's possible to combine the use of SSH and other security measures. By using SSH, it is viable to encrypt the data as it is being transferred, making it much more difficult for any unauthorized individual to access it. Additionally, it's also possible to use firewalls and other security protocols to ensure that the data is kept safe and secure during transfer. By taking these steps, the data can be protected, and the file transfer secured with confidence. Therefore, <span style="color: #2dc26b;">rsync</span> can be told to use SSH as the following:

### Secure Transfer to the Backup

```Shell
bobinsson@BoB$ rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

The data transfer between the local host and the backup server occurs over the encrypted SSH connection, which provides confidentiality and integrity protection for the data being transferred. This encryption process ensures that the data is protected from any malicious actors who would otherwise be able to access and modify the data without authorization. The encryption key itself is also safeguarded by a comprehensive set of security protocols, making it even more difficult for any unauthorized person to gain access to the data. In addition, the encrypted connection is designed to be highly resistant to any attempts to breach security, allowing us to have confidence in the protection of the data being transferred.

&nbsp;

## Auto-Synchronization

To enable auto-synchronization using <span style="color: #2dc26b;">rsync</span>, you can use a combination of <span style="color: #2dc26b;">cron</span> and <span style="color: #2dc26b;">rsync</span> to automate the synchronization process. Scheduling the cron job to run at a regular intervals ensures that the contents of the two systems are kept in sync. This can be especially beneficial for organizations that need to keep their data synchronized across multiple machines. Furthermore, setting up auto-synchronization with <span style="color: #2dc26b;">rsync</span> can be a great way to save time and effort, as it eliminates the need for manual synchronization. It also helps to ensure that the files and data stored in the systems are kept up-to-date and consistent, which helps to reduce errors and improve efficiency.

Here is an example to demonstrate automatic synchronization by using scripts. Creating a new script called <span style="color: #2dc26b;">RSYNC_Backup.sh</span>, that will trigger the <span style="color: #2dc26b;">rsync</span> command to sync the local directory with the remote one:

### RSYNC_Backup.sh

```Shell
#!/bin/bash

rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

Then, in order to ensure that the script is able to execute properly, the necessary permissions must be provided. Additionally, it is also important to make sure that the script is owned by the correct user, as this will ensure that only the correct user has access to the script and that the script is not tampered with by any other user.

```Shell
bobinsson@BoB$ chmod +x RSYNC_Backup.sh
```

### Crontab

After that, the crontab that tells <span style="color: #2dc26b;">cron</span> to run the script every hour at the 0th minute can be created. The timing can be adjusted according to the needs. To do so, the crontab needs the following content:

```Shell
0 * * * * /path/to/RSYNC_Backup.sh
```

With this setup, <span style="color: #2dc26b;">cron</span> will be responsible for executing the script at the desired interval, ensuring that the <span style="color: #2dc26b;">rsync</span> command is run and the contents of the local directory are synchronized with the remote host.