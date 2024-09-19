# Finding Files and Directories

Commands that allow for quick and practical search inside the linux file system.

### which

Returns the path of the file or link that should be executed. This allows to determine if specific programs, like cURL, netcat, wget, python, gcc, are available on the operating system.

ex:

```Shell
bobinsson@BoB$ which python
/usr/bin/python
```

&nbsp;

### find

Besides the function to find files and folders, this tool also contains the function to filter the results. We can use filter parameters like the size of the file or the date. We can also specify if we only search for files or folders.

ex:

```Shell
bobinsson@BoB$ find <location> <options>
bobinsson@BoB$ find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null
```

| \-type f | defines the type of the searched object. In this case, f stands for "file" |
| --- | --- |
| \-name \*.config | indicates the name of the file being looked for. The asterisk (\*) stands for "all" files that ends with the ".conf" extension |
| \-user root | filters all files whose owner is not the root user |
| \-size +20k | filter all the files that are larger than 20KiB |
| \-newermt 2020-03-03 | sets the date, only files newer than the specified date will be presented |
| \-exec ls -al {} \\; | executes the specified command, using the curly brackets as placeholders for each result. The backslash escapes the next character from being interpreted by the shell because otherwise, the semicolon would terminate the command and not reach the redirection |
| 2>/dev/null | STDERR redirection to the "null device", ensuring that no errors are displayed in the terminal. Must not be an option of the find command |

&nbsp;

&nbsp;

### locate

Offers a quicker way to search through the system. In contrarst to the "find" commaand, "locate" works with a local database that contains all information about existing files and folders. The database can be update using the following command:

```Shell
bobinsson@BoB$ sudo updatedb
```

Searching for all files with the ".conf" extension should produce results much faster than using "find".

```Shell
bobinsson@BoB$ locate *.conf
/etc/GeoIP.conf
/etc/NetworkManager/NetworkManager.conf
/etc/UPower/UPower.conf
/etc/adduser.conf
<SNIP>
```

However, this tool does not have many filter options that we can use. So it is always worth considering whether to use "locate" command or instead use the "find" command.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Questions

- What is the name of the config file that has been created after 2020-03-03 and is smaller than 28k but larger than 25k?

&nbsp;

- How many files exist on the system that have the ".bak" extension?

&nbsp;

- Submit the full path of the "xxd" binary.