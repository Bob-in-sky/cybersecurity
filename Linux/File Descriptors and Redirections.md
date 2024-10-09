# File Descriptors and Redirections

A file descriptor (FD) in Unix/Linux operating systems is an indicator of connection maintained by the kernel to perform I/O operation. In Windows-based OS, it's called filehandle. It is the connection (generally to a file) from the Operating System to perform I/O operations. By default, the first three file descriptors in Linux are:

- Data Stream for input: <span style="color: #2dc26b;">STDIN - 0</span>
- Data Stream for output: <span style="color: #2dc26b;">STDOUT - 1</span>
- Data Stream for output that relates to an error ocurring: <span style="color: #2dc26b;">STDERR - 2</span>

&nbsp;

&nbsp;

### STDIN & STDOUT

When running cat, we give the running program our standard input (STDIN - FD 0), wherein this case "SOME TEXT" is. As soon as we confirm the input with \[ENTER\], it is returned to the terminal as standard output (STDOUT - FD 1).

```Shell
bobinsson@BoB$ cat
SOME TEXT	(STDIN)
SOME TEXT	(STDOUT)
```

&nbsp;

&nbsp;

### STDOUT & STDERR

By using find command, we will see the standard output (STDOUT - FD 1) and standard error (STDERR - FD 2).

```Shell
bobinsson@BoB$ find / -name root
/mnt/c/dev/flutter/dev/bots/test/analyze-test-input/root
/mnt/c/Program Files/Microsoft Office/root
find: ‘/mnt/c/ProgramData/Microsoft/Windows/CapabilityAccessManager’: Permission denied
find: ‘/mnt/c/ProgramData/Microsoft/Windows/SystemData’: Permission denied
find: ‘/mnt/c/ProgramData/Microsoft/Windows Defender/DLPCache/SensitiveBuffer’: Permission denied

```

In this case, the error is marked and displayed with "Permission denied". We can check this by redirecting the file descriptor for the errors (FD 2 - STDERR) to "/dev/null". This way, we redirect the resulting errors to the "null device", which discards all the data.

```Shell
bobinsson@Bob$ find / -name root 2>/dev/null
/mnt/c/dev/flutter/dev/bots/test/analyze-test-input/root
/mnt/c/Program Files/Microsoft Office/root
```

&nbsp;

&nbsp;

### Redirect STDOUT to a file

Now we can see that all errors (STDERR) previously presented with "Permission denied" are no longer displayed. The only result we see now is the standard output (STDOUT), which we can also redirect to a file with the name results.txt that will only contain standard output without the standard errors.

```Shell
bobinsson@BoB$ find / -name root 2>/dev/null > results.txt
bobinsson@BoB$ cat results.txt
/mnt/c/dev/flutter/dev/bots/test/analyze-test-input/root
/mnt/c/Program Files/Microsoft Office/root

```

&nbsp;

&nbsp;

### Redirect STDOUT and STDERR to separate files

In the previous example no number was placed before the greater-than (>) sign. Since all the errors where redirected to "null device" previously, the only output acquired was the stanndart ooutpu (FD 1 - STDOUT). To make this more precise, we will redirect standard error (FD 2 - STDERR)  and standard output (FD 1 -STDOUT) to different files.

```Shell
bobinsson@BoB$ find / -name root 2>stderr.txt 1>stdout.txt
bobinsson@BoB$ cat stdout.txt
/mnt/c/dev/flutter/dev/bots/test/analyze-test-input/root
/mnt/c/Program Files/Microsoft Office/root
bobinsson@BoB$ cat stderr.txt
find: ‘/mnt/c/ProgramData/Microsoft/Windows/CapabilityAccessManager’: Permission denied
find: ‘/mnt/c/ProgramData/Microsoft/Windows/SystemData’: Permission denied
find: ‘/mnt/c/ProgramData/Microsoft/Windows Defender/DLPCache/SensitiveBuffer’: Permission denied
```

&nbsp;

&nbsp;

### Redirect STDIN

In combination with file descriptors, we can redirect errors and output with the greater-than character (>). This also works with the lower-than (<) sign. However, the lower-than sign serves as standard input (FD 0 - STDIN). These characters can be seen as "direction" in the form of an arrow that tells us "from where" and "where to" the data should be redirected. We use the cat command to use the contents of the file stdout.txt as STDIN.

```Shell
bobinsson@BoB$ cat < stdout.txt
/mnt/c/dev/flutter/dev/bots/test/analyze-test-input/root
/mnt/c/Program Files/Microsoft Office/root
```

&nbsp;

&nbsp;

### Redirect STDOUT and append to a file

When using the greater-than (>) sign to redirect STDOUT, a new file is automatically created if it does not already exist. If the file exists, it will be overwritten without asking for confirmation. If we want to append STDOUT to our existing file, we can use the double greater-than (>>) sign.

```Shell
bobinsson@BoB$ find /etc/ -name passwd >> stdout.txt 2>/dev/null
bobinsson@BoB$ cat stdout.txt
/mnt/c/dev/flutter/dev/bots/test/analyze-test-input/root
/mnt/c/Program Files/Microsoft Office/root
/etc/pam.d/passwd
/etc/passwd

```

&nbsp;

&nbsp;

### Redirect STDIN stream to a file

The double lower-than (<<) sign can be used to append the standard input through a stream. We can use the so called End Of File (EOF) function of a Linux system file, which defines the input's end. In the next example we will use the cat command to read our streaming input through the stream and direct it to aa file.

```Shell
bobinsson@BoB$ cat << EOF > stream.txt
> Hoi!
> It me
> BoB!
> EOF
bobinsson@BoB$ cat stream.txt
Hoi!
It me
BoB!
```

&nbsp;

&nbsp;

### Pipes

Another way to redirect STDOUT is to use pipes (|). These are useful when we want to use the STDOUT from one program to be processed by another. One of the most commonly used tool is grep, which we will use in the next example. Grep is used to filter STDOUT according to the pattern we define. In the next example, we use the find command to search for all files in the "/etc/" directory with a ".conf" extension. Any error are redirected to the "null device" (/dev/null). Using grep, we filter out the results and specify that only the lines containing the patter "systemd" should be displayed.

```Shell
bobinsson@BoB$ find /etc/ -name *.conf 2>/dev/null | grep systemd
/etc/systemd/pstore.conf
/etc/systemd/sleep.conf
/etc/systemd/networkd.conf
/etc/systemd/logind.conf
/etc/systemd/journald.conf
/etc/systemd/system.conf
/etc/systemd/user.conf
```

The redirections work, not only once. We can use the obtained results to redirect them to another program. For the next example, we will use the tool called wc, which should count the total number of obtained results.

```Shell
bobinsson@BoB$ find /etc/ -name *.conf 2>/dev/null | grep systemd | wc -l
7
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## Questions

- How many files exist on the system that have the ".log" file extension?

&nbsp;

- How many total packages are installed on the target system?