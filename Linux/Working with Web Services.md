# Working with Web Services

Another essential component is the communication with the web servers. There are many different ways to set up web servers on Linux operating systems. One of the most used and widespread web servers, besides IIS and Nginx, is Apache. For an Apache web server, we can use appropriate modules, which can encrypt the communication between browser and web server (mod_ssl), use a proxy server (mod_proxy), or perform complex manipulations on HTTP header data (mod_headers) and URLs (mod_rewrite).

Apache offers the possibility to create web pages dynamically using server-sided scripting languages. Commonly used scripting languages are PHP, Perl, Ruby. Other languages are Python, JavaScript, Lua, and .NET, which can be used for this. We can install the Apache web server with the following command.

```Shell
bobinsson@BoB$ apt install apache2 -y

Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  apache2-doc apache2-suexec-pristine | apache2-suexec-custom
The following NEW packages will be installed:
  apache2
0 upgraded, 1 newly installed, 0 to remove and 17 not upgraded.
Need to get 95,1 kB of archives.
After this operation, 535 kB of additional disk space will be used.
Get:1 http://de.archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2 amd64 2.4.29-1ubuntu4.13 [95,1 kB]
Fetched 95,1 kB in 0s (270 kB/s)   
<SNIP>
```

After having started it, it's possible to navigate to the default page (http://localhost) by using the browser.

![image](../_resources/apache-default.png)

<span style="color: #a4b1cd;">This is the default page after installation and serves to confirm that the webserver is working correctly.</span>

## CURL

<span style="color: #a4b1cd;"><span style="color: #2dc26b;">cURL</span> is a tool that allows to transfer files from the shell over protocols like <span style="color: #2dc26b;">HTTP</span>, <span style="color: #2dc26b;">HTTPS</span>, <span style="color: #2dc26b;">FTP</span>, <span style="color: #2dc26b;">SFTP</span>, <span style="color: #2dc26b;">FTPS</span>, or <span style="color: #2dc26b;">SCP</span>. This tool gives possibility to control and test websites remotely. Besides the remote servers' content, it's also possible to view individual requests to look at the client's and server's communication. Usually, <span style="color: #2dc26b;">cURL</span> is already installed on most Linux systems. This is another critical reason to familiarize with the tool, as it can make some processes much easier later on.</span>

```Shell
bobinsson@BoB$ curl http://localhost

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <!--
    Modified from the Debian original for Ubuntu
    Last updated: 2016-11-16
    See: https://launchpad.net/bugs/1288690
  -->
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
    <style type="text/css" media="screen">
...SNIP...
```

In the title tag, there is the same text as from the browser. This allows us to inspect the source code of the website and get information from it. Nevertheless, we will come back to this in another module.

&nbsp;

## wget

An alternative to cURL is the tool <span style="color: #2dc26b;">wget</span>. With this tool, it's possible to download files from FTP or HTTP servers directly from the terminal, and it serves as a good download manager. When using wget in the same way, the difference to curl is that the website content is downloaded and stored locally, as shown in the following example. 

```Shell
bobinsson@BoB$ wget http://localhost

--2020-05-15 17:43:52--  http://localhost/
Resolving localhost (localhost)... 127.0.0.1
Connecting to localhost (localhost)|127.0.0.1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10918 (11K) [text/html]
Saving to: 'index.html'

index.html                 100%[=======================================>]  10,66K  --.-KB/s    in 0s      

2020-05-15 17:43:52 (33,0 MB/s) - ‘index.html’ saved [10918/10918]
```

&nbsp;

## Python 3

Another option that is often used when it comes to data transfer is the use of Python 3. In this case, the web server's root directory is where the command is executed to start the server. For this example, WordPress is installed in a directory and it contains a "readme.html" file. Now, the Python 3 web server is installed and attempted to be accessed through the browser.

```Shell
bobinsson@BoB$ python3 -m http.server

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

![image](../_resources/python3-browser.png)

&nbsp;

&nbsp;It's possible to see which requests are made when looking at the Python 3 web server's events.

```Shell
bobinsson@BoB$ python3 -m http.server

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
127.0.0.1 - - [15/May/2020 17:56:29] "GET /readme.html HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/css/install.css?ver=20100228 HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.png HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.svg?ver=20131107 HTTP/1.1" 200 -
```

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# Questions

- Find a way to start a simple HTTP server using "npm". Submit the command that starts the web server on port 8080 (use the short argument to specify the port number).

http-server -p 8080

&nbsp;

- Find a way to start a simple HTTP server using "php". Submit the command that starts the web server on localhost (127.0.0.1) port 8080.

php -S 127.0.0.1:8080