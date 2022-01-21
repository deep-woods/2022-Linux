# [REPOSITORY]

[REPOSITORY] learn forest

<br>

### Notes

Cool stuff to learn: Linux, Docker, Kubernetes, AWS! 

<br>

### <span id="top">Content</span>

[[Setup]](#setup)   
[[Basics]](#basics)   
[[Commands]](#command)s  
[[Pakcage Managers]](#package)  
[[Services]](#services)  
[[VirtualBox]](#virtualbox)  
[[Networking]](#networking)  
[[]](#)  
[[References]](#ref)   


<br>

### <span id='setup'>Setup</span>

[[☝️top]](#top)

<br>

## <span id="basics">Basics</span>

[[☝️top]](#top)

Things covered:

- Vinux CLI
- VI Editor
- [Package Management](#package)  
- Service Management

CentOS is a free Red hat-developed community version of Linux. 

<br>


## <span id="commands">Commands</span>

[[☝️top]](#top)

### Basic operations

- `echo $SHELL`: check your shell version.
  - `/bin/bash`
- `echo`: print to screen
- `command 1; command 2; command 3; ...`: multiple commands

- `ls`
- `cd`
- `pwd`
- `mkdir`
  - `mkdir -p /tmp/asia/india/bangalore`
- `rm -r`
- `cp -r file_1 /new_path/file_1`: copy file

<br>

### Files

- `touch new_file.txt`: creates a new file (no content)
- `cat > new_file.txt`: concatenates content to file
- `cat new file.txt`: view content of file
  
      input
      ctrl + D

- `cp new_file.txt ./new_path/new_file.txt`: copy
- `mv old_path /new_path`: move file to a different location.

<br>

### User accounts

- `whoami`: Check the current account name
- `id`
- `su user_name`: switch user
- `ssh user_name@xxx.xxx.xx.xx`: switch user and system.

- `ls /root`
  - `ls: cannot open directory '/root': Permission denied`

<br>

### Download files

- `curl https://url -O`: the Capital `-O` at the end saves the url content to a file in the current location.
- `wget https://url -O file.txt`

<br>

### Check OS versionls 

- `ls /etc/*release*`: check your OS version.

      `/etc/lsb-release  /etc/os-release`

- `cat /etc/*release*`: check more detail

      DISTRIB_ID=Ubuntu
      DISTRIB_RELEASE=20.04
      DISTRIB_CODENAME=focal
      DISTRIB_DESCRIPTION="Ubuntu 20.04.3 LTS"
      NAME="Ubuntu"
      VERSION="20.04.3 LTS (Focal Fossa)"
      ID=ubuntu
      ID_LIKE=debian
      PRETTY_NAME="Ubuntu 20.04.3 LTS"
      VERSION_ID="20.04"

<br>

## <span id="package">Package Managers</span>

[[☝️top]](#top)

### RPM (Red Hat Package Manager)

- `rpm -i telnet.rpm`: install package
- `rpm -e telnet.rpm`: uninstall package
- `rpm -q telnet.rpm`: query package

<br>

- `rpm -q openssh-server python3 ansible telnet`: (-q = query)
- `sudo rpm -i /opt/ftp-0.17-67.el7.x86_64.rpm`
- `sudo rpm -e /opt/ftp-0.17-67.el7.x86_64`

### YUM

a high-level package manager that uses RPM underneath.

The Yellowdog Updater, Modified is a free and open-source command-line package-management utility for computers running the Linux operating system using the RPM Package Manager. 

- `yum repolist`
- `ls /etc/yum.repos.d`
- `cat /etc/yum.repos.d/CentOS-Base.repo`

- `yum list ansible`
- `yum remove ansible`
- `yum --showduplicates list ansible`
- `yum instal ansible-2.4.2.0`

<br>

- `sudo yum install ansible`

      Installed:
        ansible.noarch 0:2.9.25-1.el7                                                                                    

      Dependency Installed:
        PyYAML.x86_64 0:3.10-11.el7                               libyaml.x86_64 0:0.1.4-11.el7_0                       
        make.x86_64 1:3.82-24.el7                                 openssl.x86_64 1:1.0.2k-24.el7_9                      
        python-babel.noarch 0:0.9.6-8.el7                         python-cffi.x86_64 0:1.6.0-5.el7                      
        python-enum34.noarch 0:1.0.4-1.el7                        python-idna.noarch 0:2.4-1.el7                        
        python-jinja2.noarch 0:2.7.2-4.el7                        python-markupsafe.x86_64 0:0.11-10.el7                
        python-paramiko.noarch 0:2.1.1-9.el7                      python-ply.noarch 0:3.4-11.el7                        
        python-pycparser.noarch 0:2.14-1.el7                      python-six.noarch 0:1.9.0-2.el7                       
        python2-cryptography.x86_64 0:1.7.2-2.el7                 python2-httplib2.noarch 0:0.18.1-3.el7                
        python2-jmespath.noarch 0:0.9.4-2.el7                     python2-pyasn1.noarch 0:0.1.9-7.el7                   

      Dependency Updated:
        openssl-libs.x86_64 1:1.0.2k-24.el7_9                                                                            

      Complete!

<br>

### Questions

Q. Which version of ansible is installed with yum _**on host01 server**_ in previous step?

`2.9.25`

<br>

Q. Remove `ansible` with yum.

      sudo yum remove -y ansible

Q. Install `ansible` version 2.8.11.

      sudo yum install -y ansible-2.8.11

<br>

## <span id="services">Services</span>

[[☝️top]](#top)


### `Httpd`

HTTPd is a software program that usually runs in the background, as a process, and plays the role of a server in a client-server model using the HTTP and/or HTTPS network protocol(s).

The process waits for the incoming client requests and for each request it answers by replying with requested information, including the sending of the requested web resource, or with an HTTP error message.

HTTPd stands for Hypertext Transfer Protocol daemon.

It usually is the main software part of an HTTP server better known as a web server.[

<br>

### Daemon 

In multitasking computer operating systems, a daemon is a computer program that runs as a background process, rather than being under the direct control of an interactive user. 

<br>

### Service Commands

`systemctl` command is the key management tool for init system control.

- `service httpd start`: start HTTPD service
- `systemctl start httpd`: start HTTPD service
- `systemctl stop httpd`: stop HTTPD service
- `systemctl status httpd`: check HTTPD service status

      ● httpd.service - The Apache HTTP Server
        Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
        Active: inactive (dead)
          Docs: man:httpd(8)
                man:apachectl(8)

- `systemctl enable httpd`: configure HTTPD to start on reboot

      Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.

- `systemctl disable httpd`: configure HTTPD to not start on reboot

- `systemctl start my_app`
- `systemctl stop my_app`

      my_app.service

      Unit configuration file:
      
      [Service]
      ExecStart=/usr/bin/pytohn3 /opt/code/my_app.py

- `systemctl deamon-reload`: let the system D know there is a new service configured.
- `systemctl start my_app`
- `systemctl status app_name`

      ● app.service - My python web application
      Loaded: loaded (/usr/lib/systemd/system/app.service; disabled; vendor preset: disabled)
      Active: inactive (dead)

<br>

      systemctl status app.service

      ● app.service - My python web application
      Loaded: loaded (/usr/lib/systemd/system/app.service; disabled; vendor preset: disabled)
      Active: inactive (dead)


### Questions

Q. You decide to use dedicated python flask app instead of apache and want to stop the `httpd` service. What do you do?

- The Apache HTTP Server ("httpd") was launched in 1995 and it has been the most popular web server on the Internet since April 1996.


      sudo systemctl stop httpd


Q. Enable/disable `httpd` service so it does/doesn't auto-start on boot. 

      sudo systemctl enable httpd

      Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.

<br>

      sudo systemctl disable httpd
      
      Removed symlink /etc/systemd/system/multi-user.target.wants/httpd.service.

Q. Is the service running? 

`/usr/lib/systemd/system/app.service`

      systemctl status app

      ● app.service - My python web application
      Loaded: loaded (/usr/lib/systemd/system/app.service; disabled; vendor preset: disabled)
      Active: inactive (dead)


Q. Look at the service configuration (in `systemd` unit file). Figure out the order of service execution. 

You want to check the `app.service` file. 

- `cat [FULL_PATH]`
- `systemctl cat [FILE_NAME]`

And look for: `ExecStart`, `ExecStartPre`, `ExecStartPost`

      cat /usr/lib/systemd/system/app.service
      systemctl cat /usr/lib/systemd/system/app.service

<br>

      app.service

      [Unit]
      Description=My python web application

      [Service]
      ExecStart=/usr/bin/python3 /opt/code/my_app.py
      ExecStartPre=/bin/bash /opt/code/configure_db.sh
      ExecStartPost=/bin/bash /opt/code/email_status.sh
      Restart=always

      [Install]
      WantedBy=multi-user.target



Q. Once everything is configured, set the app to auto-start on reboot. 

      sudo systmctl enable app

      Created symlink from /etc/systemd/system/multi-user.target.wants/app.service to /usr/lib/systemd/system/app.service.


<br>

## <span id="virtualbox">VirtualBox</span>

[[☝️top]](#top)

- Deploying VMs
- Multiple VMs
- Networking and troubleshooting network
- Snapshots and restore VMs

<br>

### Virtualisation S/W

Type 1: VMware, Microsoft Hyper-v
Type 2: Oracle VirtualBox, VMWare Workstation
  - Host is the operating system. 

<br>

### Setup

1. Install `VirtualBox` at https://www.virtualbox.org/wiki/Downloads.

- The VM itself and its disks are installed on the host OS.
- Creating a virtual machine is like buying a new computer wihtout an OS installed in it. 

<br>

## <span id="networking">Networking</span>

[[☝️top]](#top)

- Switching
- Routing
- Default Gateway
- DNS Configuration on Linux

There are two systems (hardware devices). How do they actually connect with and talk to each other? We need an `interface` for that. 

<br>

`Switching`: Inter-system networking setup

| system 1   |  system 2  |
| -- | -- |
| `ip link`   | `ip link`   |
| `ip addr add 192.168.1.10/24 dev eth0`   |  `ip addr add 192.168.1.11/24 dev eth0`  |
| `ping 192.168.1.11`   |    |

<br>

`Routing`: Inter-network networking setup

- Each network gets assigned an IP. 
- How does the system 1 know where the router is to send the packets through to communicate with the system 3?
- The router is just another device in the network. 
- And we **configure** the system's `gateway` or `route`.
- If a network is a room, a gateway is a door to the outside world (internet). 
  - The systems need to know where they need to go to go through it. 

| system 1 in network A  |  system 3 in network B |
| -- | -- |
| `ip link`   | `ip link`   |
| `ip addr add 192.168.1.10/24 dev eth0`   |  `ip addr add 192.168.1.11/24 dev eth0`  |
| `ping 192.168.1.11`   |    |

      ip route add 192.168.2.0/24 via 192.168.1.1
      
      route
      Destination       Gateway           Genmask           Iface
      192.168.2.0       192.168.1.1       255.255.255.0     eth0


<br>

## <span id=""></span>

[[☝️top]](#top)

<br>



## <span id=""></span>

[[☝️top]](#top)

<br>


## <span id=""></span>

[[☝️top]](#top)

<br>

### <span id="ref">References</span>

[[☝️top]](#top)

  - freeCodeCamp.org (2020) DevOps Prerequisites Course - Getting started with DevOps https://www.youtube.com/watch?v=Wvf0mBNGjXY