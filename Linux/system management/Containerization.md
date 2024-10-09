# Containerization

Containerization is a process of packaging and running applications in isolated environments, such as a container, virtual machine, or serverless environment. Technologies like Docker, Docker Compose, and Linux Containers make this process possible in Linux systems. These technologies allow users to create, deploy, and manage applications in various ways, allowing the to tailor the application to their needs. Additionally, containers are incredibly lightweight, perfect for running multiple applications simultaneously and providing scalability and portability. Containerization is a great way to ensure that applications are managed and deployed efficiently and securely.

Container security is an important aspect of containerization. They provide users as secure environment for running their applications since they are isolated from the host system and other containers. This isolation helps protect the host system from any malicious activities in the container while providing an additional layer of security for the applications running on the containers. Additionally, containers have the advantage of being lightweight, which makes them more difficult to compromise than traditional virtual machines. Furthermore, containers are easy to configure, making them ideal for running applications securely.

In addition to providing a secure environment, containers provide users with many other advantages because they make applications easier to deploy and manage and are efficient for running multiple applications simultaneously. However, methods still exist to escalate privileges on containers and escape those.

&nbsp;

## Dockers

Docker is an open-source platform for automating the deployment of applications as self-contained units called containers. It uses a layered file system and resource isolation features to provide flexibility and portability. Additionally, it provides a robust set of tools for creating, deploying, and managing applications, which helps streamline the containerization process.

### Install Docker-Engine

Installing Docker is relatively straightforward, the following script can be used to install it on a Ubuntu host:

```script
#!/bin/bash

# Preparation
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Add user bobinsson to the Docker group
sudo usermod -aG docker bobinsson
echo '[!] You need to log out and log back in for the group changes to take effect.'

# Test Docker installation
docker run hello-world
```

The Docker engine and specific Docker images are needed to run a container. These can be obtained from the Docker Hub (https://hub.docker.com/), a repository of pre-made images, or created by the user. The Docker Hub is a cloud-based registry for software repositories or a library for Docker images. It is divided into a <span style="color: #2dc26b;">public</span> and a <span style="color: #2dc26b;">private</span> area. The public area allows users to upload and share images with the community. It also contains official images from the Docker development team and established open-source projects. Images uploaded to a private area of the registry are not publicly accessible. They can be shared within a company or with teams and acquaintances.

### Dockerfile

Creating a Docker image is done by creating a Dockerfile (https://docs.docker.com/engine/reference/builder/), which contains all the instructions the Docker engine needs to create the container. Docker containers can be used as a "file hosting" server when transferring specific files to a target systems. Therefore, a <span style="color: #2dc26b;">Dockerfile</span> must be created based on Ubuntu 22.04 with <span style="color: #2dc26b;">Apache</span> and <span style="color: #2dc26b;">SSH</span> server running. With this, it will be possible to use <span style="color: #2dc26b;">scp</span> to transfer files to the docker image, and Apache allows to host files and use tools like <span style="color: #2dc26b;">curl</span>, <span style="color: #2dc26b;">wget</span>, and others on the target system to download the required files. Such a <span style="color: #2dc26b;">Dockerfile</span> could look like the following:

```script
# Use the latest Ubuntu 22.04 LTS as the base image
FROM ubuntu:22.04

# Update the package repository and install the required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user called "student"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Give the htb-student user full access to the Apache and SSH services
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose the required ports
EXPOSE 22 80

# Start the SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

### Docker build

After defining the Dockerfile, it is necessary to convert it into an image. With the <span style="color: #2dc26b;">build</span> command, take the directory with the <span style="color: #2dc26b;">Dockerfile</span>, execute the steps from the Dockerfile, and store the image in the local Docker Engine. If one of the steps fails due to an error, the container creation will be aborted. With the option <span style="color: #2dc26b;">\-t</span>, the container is given a tag, so it's easier to identify and work with later.

```Shell
bobinsson@BoB$ docker build -t FS_docker <directory_of_docker_file>
```

### Docker run

Once the Docker image has been created, it can be executed through the Docker engine, making it a very efficient and easy way to run a container. It is similar to the virtual machine concept, based on images. Still, these images are read-only templates and provide the file system necessary for runtime and all parameters. A container can be considered a running process of an image. When a container is to be started on a system, a package with the respective image is first loaded if unavailable locally. The container can be started by the command docker run (https://docs.docker.com/engine/reference/commandline/run/):

```Shell
bobinsson@BoB$ docker run -p <host port>:<docker port> -d <docker container name>

bobinsson@BoB$ docker run -p 8022:22 -p 8080:80 -d FS_docker
```

In this case, the container is started from the image <span style="color: #2dc26b;">FS_docker</span> and maps the host ports 8022 and 8080 to container ports 22 and 80, respectively. The container runs in the background, allowing access to SSH and HTTP services inside the container using the specified host ports.

### Docker Management

When managing Docker containers, Docker provides a comprehensive suite of tools that enables to easily create, deploy, and manage containers. With these powerful tools, the containers can be listed, started, stopped, and effectively managed, ensuring seamless execution of applications. Some of the most commonly used Docker management commands are:

| Command | Description |
| --- | --- |
| <span style="color: #2dc26b;">docker ps</span> | List all running containers |
| <span style="color: #2dc26b;">docker stop</span> | Stop a running container |
| <span style="color: #2dc26b;">docker start</span> | Start a stopped container |
| <span style="color: #2dc26b;">docker restart</span> | Restart a running container |
| <span style="color: #2dc26b;">docker rm</span> | Remove a container |
| <span style="color: #2dc26b;">docker rmi</span> | Remove a Docker image |
| <span style="color: #2dc26b;">docker logs</span> | View the logs of a container |

It is worth noting that these commands, used in Docker, can be combined with various options to provide additional functionality. For example, the exposed ports can be specified, volumes can be mounted, or environment variables can be set. This allows to customize the Docker containers to suit needs and requirements. When working with Docker images, it is important to note that any changes made to an existing image are not permanent. Instead, we need to create a new image that inherits from the original and includes the desired changes.

This is done by creating a new Dockerfile that starts with the <span style="color: #2dc26b;">FROM</span> statement, which specifies the base image, and then adds the necessary commands to make the desired changes. Once the Dockerfile is created, the <span style="color: #2dc26b;">docker build</span> command can be used to build the new image, tagging it with a unique name to help identify it. This process ensures that the original image remains intact while allowing to create a new image with the desired changes.

It is important to note that Docker containers are designed to be immutable, meaning that any changes made to a container during runtime are lost when the container is stopped. Therefore, it is recommended to use container orchestration tools such as Docker Compose or Kubernetes to manage and scale containers in a production environment

&nbsp;

## Linux Containers

Linux Containers (<span style="color: #2dc26b;">LXC</span>) is a virtualization technology that allows multiple isolated Linux systems to run on a single host. It uses resource isolation feature, such as <span style="color: #2dc26b;">cgroups</span> and <span style="color: #2dc26b;">namespaces</span>, to provide a lightweight virtualization solution. LXC also provides a rich set of tools and APIs for managing and configuring containers, contributing to its popularity as a containerization technology. By combining the advantages of LXC with the power of Docker, users can achieve a fully-fledged containerization experience in Linux systems.

Both LXC and Docker are containerization technologies that allow for applications to be packaged and run in isolated environments. However, there are some differences between the two that can be distinguished based on the following categories:

- Approach
- Image building
- Portability
- Easy of use
- Security

LXC is a lightweight virtualization technology that uses resource isolation features of the Linux kernel to provide an isolated environment for applications. In LXC, images are manually built by creating a root file system and installing the necessary packages and configurations. Those containers are tied to the host system, may not be easily portable, and may require more technical expertise to configure and manage. LXC also provides some security features but may not be as robust as Docker.

On the other hand, Docker is an application-centric platform that builds on top of LXC and provides a more user-friendly interface for containerization. Its images are built using a Dockerfile, which specifies the base image and the steps required to build the image. Those images are designed to be portable so they can be easily moved from one environment to another. Docker provides a more user-friendly interface for containerization, with a rich set of tools and APIs for managing and configuring containers with a more secure environment for running applications.

### Install LXC

To install LXC on a Linux distribution, the distribution's package manager can be used. For example, on Ubuntu, the apt package manager can be used to install LXC with the following commaand:

```Shell
bobinsson@BoB$ sudo apt-get install lxc lxc-utils -y
```

Once LXC is installed, the containers can start being created and managed on the Linux host. It is worth noting that LXC requires the Linux kernel to support the necessary features for containerization. Most modern Linux kernels have built-in support for containerization, but some older kernels require additional configuration or patching to enable support for LXC.

### Creating an LXC Container

To create a new LXC container, we can use the <span style="color: #2dc26b;">lxc-create</span> command followed by the container's name and the template to use. For example, to create a new Ubuntu container named <span style="color: #2dc26b;">linuxcontainer</span>, the following command can be used:

```Shell
bobinsson@BoB$ sudo lxc-create -n linuxcontainer -t Ubuntu
```

### Managing LXC Containers

When working with LXC containers, several tasks are involved in managing them. These tasks include creating new containers, configuring their settings, starting and stopping them as necessary, and monitoring their performance. Fortunately, there are many command-line tools and configuration files available that can assist with these tasks. These tools enable to quickly and easily manage our containers, ensuring they are optimized for our specific needs and requirements. By leveraging these tools effectively, it can be ensured that the LXC containers run efficiently and effectively, allowing us to maximize our system's performance and capabilities.

| Command | Description |
| --- | --- |
| <span style="color: #2dc26b;">lxc-ls</span> | List all existing containers |
| <span style="color: #2dc26b;">lxc-stop -n &lt;container&gt;</span> | Stop a running container |
| <span style="color: #2dc26b;">lxc-start -n &lt;container&gt;</span> | Start a stopped container |
| <span style="color: #2dc26b;">lxc-restart -n &lt;container&gt;</span> | Restart a running container |
| <span style="color: #2dc26b;">lxc-config -n &lt;container name&gt; -s storage</span> | Manage container storage |
| <span style="color: #2dc26b;">lxc-config -n &lt;container name&gt; -s network</span> | Manage container network settings |
| <span style="color: #2dc26b;">lxc-config -n &lt;container name&gt; -s security</span> | Manage container security settings |
| <span style="color: #2dc26b;">lxc-attach -n &lt;container&gt;</span> | Connect to a container |
| <span style="color: #2dc26b;">lxc-attach -n &lt;container&gt; -f /path/to/share</span> | Connect to a container and share a specific directory or file |

As penetration testers, we may encounter situations where we must test software or systems with dependencies or configurations that are difficult to reproduce on our own machines. This is where Linux containers come in handy. Since a Linux container is a lightweight, standalone executable package containing all the necessary dependencies and configuration files to run a specific software or system, it provides an isolated environment that can be run on any Linux machine, regardless of the hosts's configuration.

Containers are useful, especially because they allow to quickly spin up an isolated environment specific to testing needs. For example, we might need to test a web application requiring a specific database or web server version. Rather than setting up these components on our machine, which can be time-consuming and error-prone, we can create a container that contains the exact configurations we need.

We can also use them to test exploits or malware in a controlled environment where we create a container that simulates a vulnerable system or network and then use that container to safely test exploits without risking damaging our machines or networks. However, it is important to configure the LXC Container security to prevent unauthorized access or malicious activities inside the container. This can be achieved by implementing several security measures, such as:

- Restricting access to the container
- Limiting resources
- Isolating the container from the host
- Enforcing mandatory access control
- Keeping the container up to date

LXC containers can be accessed using various methods, such as SSH or console. It is recommended to restrict access to the container by disabling unnecessary services, using secure protocols, and enforcing strong authentication mechanisms. For example, we can disable SSH access to the container by removing <span style="color: #2dc26b;">openssh-server</span> package or by configuring SSH only to allow access from trusted IP addresses. Those containers also share the same kernel as the host system, meaning they can access all the resources available on the system. We can use resource limits or quotas to prevent containers from consuming excessive resources. For example, we can use <span style="color: #2dc26b;">cgroups</span> to limit the amount of CPU, memory, or disk space that a container can use.

### Securing LXC

Let us limit the resources to the container. In order to configure <span style="color: #2dc26b;">cgroups</span> for LXC and limit the CPU and memory, a container can create a new configuration file in the <span style="color: #2dc26b;">/usr/share/lxc/config/&lt;container name&gt;.conf</span> directory with the name of our container. For example, to create a configuration file for a container named <span style="color: #2dc26b;">linuxcontainer</span>, we can use the following command:

```Shell
bobinsson@BoB$ sudo vim /usr/share/lxc/config/linuxcontainer.conf
```

In this configuration file, we can add the following lines to limit the CPU and memory the container can use.

```txt
lxc.cgroup.cpu.shares = 512
lxc.cgroup.memory.limit_in_bytes = 512M
```

When working with containers, it is important to understand the <span style="color: #2dc26b;">lxc.cgroup.cpu.shares</span> parameter. This parameter determines the CPU time a container can use in relation to the other containers on the system. By default, this value is set to 1024, meaning the container can use up to its fair share of CPU time. However, if we set this value to 512, for example, the container can only use up to half of the CPU time available on the system. This can be a useful way to manage resources and ensure all containers have the necessary access to CPU time.

One of the key parameters in controlling the resource allocation of a container is the <span style="color: #2dc26b;">lxc.cgroup.memory.limit_in_bytes</span> parameter. This parameter allows you to set the maximum amount of memory a container can use. It's important to note that this value can be specified in a variety of units, including bytes, kilobytes (K), megabytes (M), gigabytes (G), or terabytes (T), allowing for a high degree of granularity in defining container resource limits. After adding these two lines, we can save and close the file by typing:

- <span style="color: #2dc26b;">\[Esc\]</span>
- <span style="color: #2dc26b;">:</span>
- <span style="color: #2dc26b;">wq</span>

To apply these changes, we must restart the LXC service.

```Shell
bobinsson@BoB$ sudo systemctl restart lxc.service
```

LXC uses <span style="color: #2dc26b;">namespaces</span> to provide an isolated environment for processes, networks and file systems from the host system. Namespaces are a feature  of the Linux kernel that allows for creating isolated environments by providing an abstraction of system resources.

Namespaces are a crucial aspect of containerization as they provide a high degree of isolation for the container's processes, network interfaces, routing tables, and firewall rules. Each container is allocated a unique process ID (<span style="color: #2dc26b;">pid</span>) number space, isolated from the host system's process IDs. This ensures that the container's processes cannot interfere with the host system's processes, enhancing system stability and reliability. Additionally, each container has its own network interfaces (<span style="color: #2dc26b;">net</span>), routing tables, and firewall rules, which are completely separate from the host system's network, providing an extra layer of network security.

Moreover, containers come with their own root file system (<span style="color: #2dc26b;">mnt</span>), which is entirely different from the host system's root file system. This separation between the two ensures that any changes or modifications made within the container's file system do not affect the host system's file system. However, it is important to remember that while namespaces provide high level of isolation, they do not provide complete security. Therefore, it is always advisable to implement additional security measures to further protect the container and the host system from potential security breaches.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# Practice

Here are 9 optional exercises to practice LXC:

| Practice |
| --- |
| 1\. Install LXC on your machine and create your first container |
| 2\. Configure the network settings for your LXC container. |
| 3\. Create a custom LXC image and use it to launch a new container. |
| 4\. Configure resource limits for your LXC containers (CPU, memory, disk space). |
| 5\. Explore the <span style="color: #2dc26b;">lxc-\*</span> commands for managing containers. |
| 6\. Use LXC to create a container running a specific version of a web server (e.g., Apache, Nginx). |
| 7\. Configure SSH to your LXC containers and connect to them remotely. |
| 8\. Create a container with persistence, so changes made to the container are saved and can be reused. |
| 9\. Use LXC to test software in a controlled environment, such as a vulnerable web application or malware. |