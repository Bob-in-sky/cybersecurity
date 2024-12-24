# telnet

This section follows a theoretical practical example to explain the use of the telnet protocol between two machines, the AttackBox and the target machine.

The TELNET (Teletype Network) protocol is a network protocol for remote terminal connection. In simpler words, `telnet`, a TELNET client, allows you to connect to and communicate with a remote system and issue text commands. Although initially it was used for remote administration, we can use `telnet` to connect to any server listening on a TCP port number.

On the target virtual machine, different services are running. We will experiment with three of them:

- Echo server: This server echoes everything you send it. By default, it listens on port 7.
- Daytime server: This server listens on port 13 by default and replies with the current day and time.
- Web (HTTP) server: This server listens on <span style="color: inherit;">TCP</span> port 80 by default and serves web pages.

Before continuing, we should mention that the echo and daytime servers are considered security risks and should not be run; however, we started them explicitly to demonstrate communication with the server using `telnet`. In the terminal below, we connect to the target VM at the echo server’s TCP port number 7. To close the connection, press the `CTRL + ]` keys simultaneously.

```shell
bobinsson@BoB$ telnet 10.10.156.172 7
telnet 10.10.156.172 7
Trying 10.10.156.172...
Connected to 10.10.156.172.
Escape character is '^]'.
Hi
Hi
How are you?
How are you?
Bye
Bye
^]

telnet> quit
Connection closed.
```

In the terminal below, we use `telnet` to connect to the daytime server listening at port 13. We noticed that the connection closes once the current date and time are returned.

```shell
bobinsson@BoB$ telnet 10.10.156.172 13
Trying 10.10.156.172...
Connected to 10.10.156.172.
Escape character is '^]'.
Thu Jun 20 12:36:32 PM UTC 2024
Connection closed by foreign host.
```

Finally, let’s request a web page using `telnet`. After connecting to port 80, you need to issue the command `GET / <span style="color: inherit;">HTTP</span>/1.1` and identify the host where anything goes, such as `Host: telnet.thm`. Next, you need to press `Enter` twice so your last input line is a blank line. The output below shows the exchange. (The page has been redacted.)

**Note**: You may have to press `Enter` after sending the information in case you don’t get a response.

```shell
bobinsson@BoB$ telnet 10.10.156.172 80
Trying 10.10.156.172...
Connected to 10.10.156.172.
Escape character is '^]'.
GET / HTTP/1.1
Host: telnet.thm

HTTP/1.1 200 OK
Content-Type: text/html
[...]

Connection closed by foreign host.
```