# Filter Contents

To read files, we do not necessarily have to use an editor. There are two tools called more and less, which are very identical. These are fundamental pagers that allow us to scroll through the file in an interactive view. Let us have a look at some examples.

&nbsp;

### more

```Shell
bobinsson@BoB$ more /etc/passwd
```

After reading the content with cat and redirecting it to more, the already mentioned pager opens, and we will automatically start at the beginning of the file.

```Shell
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
--More--
```

With the \[Q\] key we can leave this pager. We will notice that the output remains in the terminal.

&nbsp;

### less

```Shell
bobinsson@BoB$ less /etc/passwd
```

Taking a look at the tool less, we will notice on the man page that it contains many more features than more. The presentation is almost the same as with more.

```Shell
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
:
```

However, when closing less with the \[Q\] key, we will notice that the output we have seen, unlike more, does not remain in the terminal.

&nbsp;

### head

Sometimes we will only be interested in specific issues either at the beginning of the file or at the end. If we only want to get the first lines of the file, we can use the tool head. By default, head prints the first ten lines of the given file or input, if not specified otherwise.

```Shell
bobinsson@BoB$ head /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
```

&nbsp;

### tail

If we only want to see the last parts of a file or results, we can use the counterpart program, called tail, which returns the last ten lines, if not specified otherwise.

```Shell
bobinsson@BoB$ tail /etc/passwd
miredo:x:115:65534::/var/run/miredo:/usr/sbin/nologin
usbmux:x:116:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
rtkit:x:117:119:RealtimeKit,,,:/proc:/usr/sbin/nologin
nm-openvpn:x:118:120:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
nm-openconnect:x:119:121:NetworkManager OpenConnect plugin,,,:/var/lib/NetworkManager:/usr/sbin/nologin
pulse:x:120:122:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
beef-xss:x:121:124::/var/lib/beef-xss:/usr/sbin/nologin
lightdm:x:122:125:Light Display Manager:/var/lib/lightdm:/bin/false
do-agent:x:998:998::/home/do-agent:/bin/false
user6:x:1000:1000:,,,:/home/user6:/bin/bash
```

&nbsp;

### sort

Depending on which results and files are dealt with, they are rarely sorted. Often it is necessary to sort the desires results alphabetically or numerically to get a better overview. For this, we can use a tool called sort.

```Shell
bobinsson@BoB$ cat /etc/passwd | sort
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
dovecot:x:114:117:Dovecot mail server,,,:/usr/lib/dovecot:/usr/sbin/nologin
dovenull:x:115:118:Dovecot login user,,,:/nonexistent:/usr/sbin/nologin
ftp:x:113:65534::/srv/ftp:/usr/sbin/nologin
games:x:5:60:games:/usr/games:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
htb-student:x:1002:1002::/home/htb-student:/bin/bash
<SNIP>
```

As we can see now, the output no longer starts with root but is now sorted alphabetically.

&nbsp;

### grep

This command is used to search for specific results that contain patterns defined by the user, offering many different features. With it, it's possible to search for users who have the default shell "/bin/bash" set as an example.

```Shell
bobinsson@BoB$ cat /etc/passwd | grep "/bin/bash"
root:x:0:0:root:/root:/bin/bash
mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
htb-student:x:1002:1002::/home/htb-student:/bin/bash
```

Another possibility is to exclude specific results. For this, the option "-v" is used with grep. In the next example, the command excludes all users who have disabled the standard shell with the name "/bin/false" or "/user/bin/nologin"

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin"
root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
postgres:x:111:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
user6:x:1000:1000:,,,:/home/user6:/bin/bash
```

&nbsp;

### cut

Specific results with different characters may be separated by delimiters. To remove specific delimiters and show the words on a line in a specific position, we can use the tool cut. By using it's option "-d" to set the delimiter to the colon (:) character, and defining with the option "-f" the position of the result in the line that we want to output.

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1
root
sync
mrb3n
cry0l1t3
htb-student
```

&nbsp;

### tr

Another possibility to replace certain characters from a line with characters defined by us, is the tool tr. As the first option, we define the character to be replaced, and as second option, we define the character for it to be replaced with. In the next example the command replaces the colon character with the blank space.

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "

root x 0 0 root /root /bin/bash
sync x 4 65534 sync /bin /bin/sync
mrb3n x 1000 1000 mrb3n /home/mrb3n /bin/bash
cry0l1t3 x 1001 1001  /home/cry0l1t3 /bin/bash
htb-student x 1002 1002  /home/htb-student /bin/bash
```

&nbsp;

### column

Since search results can often have an unclear representation, the tool column is well suited to display such results in tabular form, using the \[ -t \] option;

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t
root         x  0     0      root               /root        /bin/bash
sync         x  4     65534  sync               /bin         /bin/sync
mrb3n        x  1000  1000   mrb3n              /home/mrb3n  /bin/bash
cry0l1t3     x  1001  1001   /home/cry0l1t3     /bin/bash
htb-student  x  1002  1002   /home/htb-student  /bin/bash
```

&nbsp;

### awk

The (g)awk program allows the user to display filtered content from columns, providing text manipulation based on sequences of patterns. With this we can display the first ($1) and last ($NF) results of a line.

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'
root /bin/bash
sync /bin/sync
mrb3n /bin/bash
cry0l1t3 /bin/bash
htb-student /bin/bash
```

&nbsp;

### sed

This is an stream editor that allows manipulation of specific sections of a file or standard input. One of it's most common uses is substituting text. Here, sed looks for patterns defined by the user in the form of regular expressions (regex) and replaces them with another used defined pattern. Taking the previous example, let's say we want to replace the word "bin" with "BOB".

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/BOB/g'
root /BOB/bash
sync /BOB/sync
mrb3n /BOB/bash
cry0l1t3 /BOB/bash
htb-student /BOB/bash
```

The \[ s \] flag at the beginning stands for the substitute command. Then the pattern to be replaced is specified. After the slash (/), the pattern to be used as replacement is introduced at the third position. Finally, the \[ g \] flag stands for replacing all matches.

&nbsp;

### wc

The wc program allows the user to count lines \[ -l \], characters \[-c \] or words \[-w \] in provided input, generally the results of another program. With the \[ -l \] option, we specify that only lines are to be counted.

```Shell
bobinsson@BoB$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l
5
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Practice

Optional exercises we can use to improve our filtering skills and get more familiar with the terminal and the commands. The file we will need to work with is the /etc/passwd file on our target and we can use any shown command above. Our goal is to filter and display only specific contents. Read the file and filter its contents in such a way that we see only:

| 1\. a line that contains only a certain specified username |
| --- |
| 2\. only the usernames |
| 3\. A specified username and it's UID |
| 4\. A specifiec username and it's UID separated by a comma (,) |
| 5\. A specified username, it's UID, and the set shell separated by comma (,) |
| 6\. All usernames with their UIDs and set shell separated by comma (,) |
| 7\. All usernames with their UID and set shells separated by a comma (,) and exclude the ones that contain nologin or false |
| 8\. All usernames with their UID and set shells separated by a comma (,) and exclude the ones that contain nologin and count all lines of the filtered output |

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Questions

- How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)

&nbsp;

- <span style="color: #ffffff;">Determine what user the ProFTPd server is running under. Submit the username as the answer.</span>

&nbsp;

- <span style="color: #ffffff;">Use cURL to obtain the source code of the "https://www.inlanefreight.com" website and filter all unique paths of that domain. Submit the number of these paths as the answer.</span>