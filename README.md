# 2022 Linux
 Linux learn forest

<br>

<span id="top">Contents</span>

Core Concepts

[[Linux Kernel]](#kernel)
[[Linux Boot Process]](#boot)  
[[Runlevel]](#runlevel)  
[[Linux Commands]](#commands)  
[[File Types and Hierarchy]](#files)  
[[Package Management]](#package)  
[[Shell - working with files]](#shell)  
[[Networking]](#ntworking)  
[[Security and File Permissions]](#security)  
[[🐳Docker]](#docker)  
[[Github]](#github)  
[[`pip`]](#pip)  
[[VS Code]](#vscode)  
[[Mouse]](#mouse)  
[[]](#)  
[[]](#)  
[[]](#)  


<br>

## <span id='kernel'>Linux Kernal</span>

The core interface between a computer's hardware and its processes; The kernel is the central module of an operating system (OS). It is the part of the operating system that loads first, and it remains in main memory

- Hardwrae: memory, CPU, devices
- Some hardware-related commands:

  - `uname`: get name and information about current kernel

  - `dmesg`: print or control the kernel ring buffer

  - `udevadm`: udev management tool. udevadm expects a command and command specific options. It controls the runtime behavior of systemd-udevd, requests kernel events, manages the event queue, and provides simple debugging mechanisms.

        udevadm info --query=path --name=/dev/sda5

        udevadm monitor

        monitor will print the received events for:
        UDEV - the event which udev sends out after rule processing
        KERNEL - the kernel uevent

  - `lspci`: list PCI. PCI stands for Peripheral Component Interconnect and is an industry standard bus for attaching peripheral devices to a computer. 

        lspci
        00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (rev 01)
        00:01.0 PCI bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX AGP bridge (rev 01)
        00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 08)
        00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
        00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 08)
        00:07.7 System peripheral: VMware Virtual Machine Communication Interface (rev 10)
        00:0f.0 VGA compatible controller: VMware SVGA II Adapter
        00:10.0 SCSI storage controller: Broadcom / LSI 53c1030 PCI-X Fusion-MPT Dual Ultra320 SCSI (rev 01)
        00:11.0 PCI bridge: VMware PCI bridge (rev 02)

        ...

        00:18.7 PCI bridge: VMware PCI Express Root Port (rev 01)
        02:00.0 USB controller: VMware USB1.1 UHCI Controller
        02:01.0 Ethernet controller: Intel Corporation 82545EM Gigabit Ethernet Controller (Copper) (rev 01)
        02:02.0 Multimedia audio controller: Ensoniq ES1371/ES1373 / Creative Labs CT2518 (rev 02)
        02:03.0 USB controller: VMware USB2 EHCI Controller
        02:04.0 SATA controller: VMware SATA AHCI controller

        - Process: application running...

  - `lsblk`: lists information about all available or the specified block devices.

        NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
        loop0    7:0    0  55.5M  1 loop /snap/core18/2284
        loop1    7:1    0 247.9M  1 loop /snap/gnome-3-38-2004/87

        ...

        loop13   7:13   0  61.9M  1 loop /snap/core20/1270
        sda      8:0    0    20G  0 disk 
        ├─sda1   8:1    0   512M  0 part /boot/efi
        ├─sda2   8:2    0     1K  0 part 
        └─sda5   8:5    0  19.5G  0 part /
        sr0     11:0    1  1024M  0 rom  

  - `lshw`: lshw is a small tool to extract detailed information on the hardware configuration of the machine. It can report exact memory configuration,  firmware  version,  mainboard configuration, CPU version and speed, cache configuration, bus speed, etc. on DMI-capable x86 or IA-64 systems and on some PowerPC machines  (PowerMac  G4  is  known  to work).

        WARNING: you should run this program as super-user.
        ubuntu                      
            description: Computer
            width: 64 bits
            capabilities: smp vsyscall32
        *-core
            description: Motherboard
            physical id: 0
            *-memory
                description: System memory
                physical id: 0
                size: 4GiB
            *-cpu:0
                product: Intel(R) Core(TM) i7-10750H CPU @ 2.60GHz
                vendor: Intel Corp.
                physical id: 1
                bus info: cpu@0
                width: 64 bits

                ...

            *-pci
                description: Host bridge
                product: 440BX/ZX/DX - 82443BX/ZX/DX Host bridge
                vendor: Intel Corporation
                physical id: 100
                bus info: pci@0000:00:00.0
                version: 01
                width: 32 bits
                clock: 33MHz
                configuration: driver=agpgart-intel
                resources: irq:0

  - `lsmem`: list the ranges of available memory with their online status

        RANGE                                 SIZE  STATE REMOVABLE BLOCK
        0x0000000000000000-0x00000000bfffffff   3G online       yes  0-23
        0x0000000100000000-0x000000013fffffff   1G online       yes 32-39

        Memory block size:       128M
        Total online memory:       4G
        Total offline memory:      0B

  - `free -m`: `-m` for MB. `-g` for GB.

        total        used        free      shared  buff/cache   available
        Mem:           3893        2577         316          44         999        1052
        Swap:           923         119         803


- `sda`: Small Computer System Interface disk == SCSI Disk. First drive (`a`) of SCSI disk (`sd`).
- type disk: the whole physical disk
- type partition: a reusable disk space carved out of the physical disk.


<br>

Kerne's responsibilities

- Memory management: keep track of memory usage and storage location
- Process management
- Device drivers: mediary/interpreter between hardware and processes
- System calls and security: receive requests for service from the processes.

Characteristics 

- Monolithic
- Modular

<br>

**Kernel Version**


      uname -r
      5.13.0-27-generic

[Kernel version].[Major verison].[Minor version]-[Patch release]-[Distro specific info]

<br>

**Kernel's memory management**

|  Kernel space  | User space |
| -- | -- |
|  Kernel  |  Applications  |
|  Device drivers  |  Programmes  |
| Kernel code<br>Kernel extentions<br>Device drivers| C<br>Java<br>Python<br>Ruby<br>Docker containers|


<br>


## <span id='boot'>Linux Boot Process</span>

`BIOS POST` > `Boot loader (GRUB2)` > `Kernel initialisation` > `INIT process (systemd)`

- `BIOS POST`: power-on self test. `BIOS` runs hardware test to makesure everything runs correctly.
- `Boot loader (GRUB2)`: `BIOS` loads and executes the boot code from the boot device (located in the first sector of the hard disk).
- `Kernel initialisation`: kernel is loaded into memory and decompressed. The kernel is generally in compressed state to conserve memory.
- `INIT process (systemd)`: sets up the user space and the processes needed for the user environment.
  - `systemd`: brings the linux host to a usable state. Responsible for mounting file systems.

        ls -l /sbin/init
        lrwxrwxrwx 1 root root 20 Jan  9 20:56 /sbin/init -> /lib/systemd/systemd

<br>

## <span id='runlevel'>Linux Runlevel</span>

    $ runlevel
    N 5

3: boot into a CLI (systemd `gmultiuser.target`)
5: Boot into a graphical intercface (systemd `graphical.target`)

<br>

**Change run level**

    $ systemctl get-default
    graphical.target

    $ ls -ltr /etc/systemd/system/default.target
    $ systemctl set-default multi-user.target
    Created symlink /etc/systemd/system/default.target  →  /lib/systemd/system/multi-user.target

<br>

## <span id='files'>Linux File Types and Hierarchy</span>

**File types**

- Regular file: images, scripts, configuration/data files
- Directory: /home/Forest 
- Special files: 
  - `/dev/`: `character files`, `block files` (hard disks, RAM)
  - `links`, `socket files`, `named files`

- `sudo file /path/file.name`: check file type

      file ~/.City.txt
      America/Peru/Lima/Miraflores/City.txt: ASCII text
      
      sudo file /root/firefox.deb
      /root/firefox.deb: Debian binary package (format 2.0
      
      sudo file /root/sample_script.sh
      /root/sample_script.sh: Bourne-Again shell script, ASCII text executable

<br>

**Filesystem Hierarchy**

- `df -hP`: report file system disk space usage

        Filesystem      Size  Used Avail Use% Mounted on
        udev            1.9G     0  1.9G   0% /dev
        tmpfs           390M  1.9M  388M   1% /run
        /dev/sda5        20G   13G  5.5G  70% /
        tmpfs           2.0G  133M  1.8G   7% /dev/shm
        tmpfs           5.0M  4.0K  5.0M   1% /run/lock
        tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
        /dev/loop0       56M   56M     0 100% /snap/core18/2284
        /dev/loop1      248M  248M     0 100% /snap/gnome-3-38-2004/87
        /dev/loop2       66M   66M     0 100% /snap/gtk-common-themes/1519
        /dev/loop3       66M   66M     0 100% /snap/gtk-common-themes/1515
        /dev/loop4      219M  219M     0 100% /snap/gnome-3-34-1804/72
        /dev/loop5       56M   56M     0 100% /snap/core18/2128
        /dev/loop6       51M   51M     0 100% /snap/snap-store/547
        /dev/loop7       55M   55M     0 100% /snap/snap-store/558
        /dev/loop8       36M   36M     0 100% /snap/pinta/18
        /dev/loop9       33M   33M     0 100% /snap/snapd/12704
        /dev/loop10     219M  219M     0 100% /snap/gnome-3-34-1804/77
        /dev/loop11      44M   44M     0 100% /snap/snapd/14295
        /dev/loop12     128K  128K     0 100% /snap/bare/5
        /dev/loop13      62M   62M     0 100% /snap/core20/1270
        /dev/sda1       511M  4.0K  511M   1% /boot/efi
        tmpfs           390M   36K  390M   1% /run/user/1000

**`init` system**

- `ls -l /sbin/init`

      lrwxrwxrwx 1 root root 20 Jan  9 20:56 /sbin/init -> /lib/systemd/systemd


<br>

## <span id='package'>Package Management</span>

Package: a compressed archive that contains all the files that are required by our particular software to run.

    gimb.deb package
    └── Sofware Binaries
    └── METADATA
    └── Configuration files


<br>

**Function of `Package Managers`**

- `dpkg` (debian-based distributions)
- `apt`
- `apt-get`
- `RPM` (red hat-based distributions)
- `yum`
- `dnf` (feature-rich, for rpm systems)

Context:

There are hundreds of different Linux distributions which run different sets of tools and libraries, software, and possibly even
different Linux kernels. As a result of this, a Linux program may not run the same way
To fix this problem, packages include a manifest of dependencies or lists of programs and versions.

- Package installation 

provides a consistent and automated process of installing, upgrading, configuring, and removing packages
from the operating system.

- Ensure the integrity and authenticity of the package

verifies packages' digital certificates and checksums - this ensures that the package
is downloaded from a trusted source and safe to install.

- Grouping packages
- Managing dependencies

<br>

### `RPM` & `YUM`

### `RPM`

A low-level package manager

Modes 

- Installing
- Uninstalling
- Upgrade
- Query
- Verifying

|  installing  | `rpm -ivh telnet.rpm`   |
| -- | -- |
| uninstalling   |  `rpm -e telnet.rpm`  |
|  upgrade  |  `rpm -Uvh telnet.rpm`  |
|   query|  `rpm -q telnet.rpm`<br>`/war/lib/rpm/`  |
| verifying   |  `rpm -Vf <path to file>`  |


`RPM` does not resolve package dependencies on its own (use `yum`).

<br>

### `YUM` (Yellowdog Updater, Modified)

This high-level package manager works on RPM-based linux systems

- RPM-based distros
- software repositores
- high-level package meentment
- `automatic dependency resolution`!

|  installing  | `rpm -ivh telnet.rpm`   |
| -- | -- |
| uninstalling   |  `rpm -e telnet.rpm`  |
|  upgrade  |  `rpm -Uvh telnet.rpm`  |
|   query|  `rpm -q telnet.rpm`<br>`/war/lib/rpm/`  |
| verifying   |  `rpm -Vf <path to file>`  |

**Installation Process**

Check configured repositories (`repos.d`) for the availability of the requested package. Also chekc if any of the dependent packages are already installed in the system or if they need to be upgraded.


    Resolving Dependencies
    --> Running transaction check
    ---> Package firefox.x86_64 0:68.6.0-1.el7.centos will be installed
    --> Processing Dependency: nspr >= 4.21 for package: firefox-68.6.0-1.el7.centos.x86_64

    Dependencies Resolved

`yum` asks the user for permission (skip with `-y` tag.)

    Transaction Summary
    ========================================================
    Install  1 Package  (+88 Dependent packages)
    Upgrade             (  7 Dependent packages)

    Total size: 268 M
    Total download size: 38 M
    Is this ok [y/d/N]: y

<br>

### `DPKG` & `APT`

### `DPKG`

A low-level package manager

|  installing/upgrate  | `dpkg -i telnet.deb`   |
| -- | -- |
| uninstalling   |  `dpkg -r telnet.deb`  |
|  list  |  `dpkg -l telnet.deb`  |
|   status |  `dpkg -s telnet.deb`  |
| verifying   |  `dpkg -p <path to file>`  |

<br>

### `Apt` / `Apt-get`

a front end package manager that relies on the dpkg utility,

- `apt update`
- `apt upgrade`
- `apt edit-sources`: edit repositories.

- `apt install telnet`
- `apt remove telnet`
- `apt search telnet`
- `apt list | grep telnet`

<br>

## <span id='shell'>Shell - working with files</span>

Check file sizes.

`disk usage`, `-h` (human readable)

    du -sk 2022-Linux
    > 23860	2022-Linux
    
    du -sh 2022-Linux
    > 24M	2022-Linux
    
    ls -lh 2022-Linux
    > total 28K
    drwxrwxr-x 2 gwhocodes gwhocodes 4.0K Jan 21 05:09 '2020 DevOps Prerequisites'
    drwxrwxr-x 2 gwhocodes gwhocodes 4.0K Jan 21 07:13  images
    -rw-rw-r-- 1 gwhocodes gwhocodes  20K Jan 22 02:31  README.md

<br>

### Archiving Files - `Tar`

Tar: Tape Archive
- groups multiple files or directories into a single file - especially useful
for archiving data.

- `-c`: create
- `-f`: file (specify file name)
- `ls -ltr`
- `tar -tf`: extract from tarball
- `tar -zcf`: `-z` compress the tarball to reduce the size.

      tar -cf [dest_path].tar [source_path]
      tar -cf /home/bob/python.tar /home/bob/reptile/snake/python

- `find`

      sudo find /opt -name forest-code
      /opt/forestfoundation/forest-code

<br>

**Compressing - Uncompressing**

- `bzip2` - `bunzip2`
- `gzip`- `gunzip`
- `xz` - `unxz`


<br>

## <span id="networking">Networking</span>

    sudo apt install net-tools

    $ route

    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    default         _gateway        0.0.0.0         UG    100    0        0 ens33
    link-local      0.0.0.0         255.255.0.0     U     1000   0        0 ens33
    172.xxx.0.0      0.0.0.0        255.255.0.0     U     0      0        0 docker0
    192.xxx.xx.0    0.0.0.0         255.255.255.0   U     100    0        0 ens33

<br>

**Network Interface**

A network interface is the point of interconnection between a computer and a private or public network. A network interface is generally a network interface card (NIC), but does not have to have a physical form. Instead, the network interface can be implemented in software. For example, the loopback interface (127.0.0.1 for IPv4 and ::1 for IPv6) is not a physical device but a piece of software simulating a network interface. The loopback interface is commonly used in test environments.

- `ip link`: list and modify interfaces on the host
  - Check the local interface. 

        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
            link/ether 00:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff
            altname enp2s1
        3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default 
            link/ether 02:xx:xx:xx:xx:xx brd ff:ff:ff:ff:ff:ff

- `ip addr`: to see the IP addresses assigned to those interfaces.
- `ip addr add`: set IP addresses on the interfaces.

- `ip r`: `ip route`. check default gateway. 

      default via 172.16.238.1 dev eth0 
      172.16.238.0/24 dev eth0 proto kernel scope link src 172.16.238.187 
      172.16.239.0/24 dev eth1 proto kernel scope link src 172.16.239.187 
      172.17.0.0/16 dev eth2 proto kernel scope link src 172.17.0.4 

<br>

### Common network issues and trouble shooting technique

**Scenario*: try to access this new URL, and a connection timeout error.

**Possible causes**:

1) `Local interface` is not connected to the network.
2) The `host` (new URL) is not resolving the IP address of the host name you were trying to connect to
3) An issue with the `route` to the server
4) the `server` with connectivity issues
5) the `software` that hosts the service isn't functioning correctly.

<br>

**Trouble shooting**

1) IP connectivity
- Check the local interface by running the IP link command.
- The state is up, so this is okey.

        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        14: eth1@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
            link/ether 02:42:ac:10:ef:bb brd ff:ff:ff:ff:ff:ff link-netnsid 0
        16: eth0@if17: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
            link/ether 02:42:ac:10:ee:bb brd ff:ff:ff:ff:ff:ff link-netnsid 0
        20: eth2@if21: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
            link/ether 02:42:ac:11:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0

<br>

2) Host name
- Check if we can resolve the host name to an IP address. Is it resolving to a valid IP?
- `DNS` server's IP is `127.0.0.11`, so the DNS server is working correctly.
- `nslookup <host name>`

      nslookup forest-repo-01
      Server:         127.0.0.11
      Address:        127.0.0.11#53

      Non-authoritative answer:
      *** Can't find forest-repo-01: No answer

<br>

3) Ping

    sudo ping forest-repo-01
    ping: unknown host

<br>

In this case.. it is most likely the problem lies between local system and the server. Something is wrong along the route between local system and the server.


<br>

4) traceroute

Shows the number of hops or devices between the source (the laptop, and the repo server)

- `sudo apt install traceroute`
- `traceroute 192.168.2.5`: the problem is between the second router and the server. 

    traceroute 192.168.2.5
    trace-routing to 192.168.2.5 (192.168.2.5), 30 hops max, 60 byte packets
    1  172.16.238.1 (172.16.238.1)  0.068 ms  0.015 ms  0.009 ms
    2  172.25.0.1 (172.25.0.1)  0.049 ms  0.013 ms  0.012 ms
    3  * * *    Requset timed out.


5) Port and HTTP process
Checks if the HTTP process is running on port 80.

- `netstat -an | grep 80 | grep -i LISTEN`
- print the information of network connections, routing tables and several other network statistics.
- `8080` is up and running (`LISTENING`)

    tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN     
    unix  2      [ ACC ]     STREAM     LISTENING     44251680 /run/systemd/journal/stdout

<br>

**Analysis**

If the host werver's network interface (`eth1`) is `DOWN`, external clients are unable to connect to it. 

    14: eth1@if15: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
        link/ether 02:42:ac:10:ef:bb brd ff:ff:ff:ff:ff:ff link-netnsid 0

Enable the interface by running: 

    ip link set dev eth1 up


<br>

## <span id="security">Security and File Permissions</span>










<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<br>

## <span id=""></span>

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## <span id=""></span>

<br>



## <span id=""></span>

<br>



## <span id=""></span>

<br>

## <span id=''>Linux </span>

<br>

## <span id='commands'>Linux Commands</span>

Internal commands: built-in. Comes bundled with the `shell`.

    echo, cd, pwd, set (about 30 commands)

External commands: binary programmes or scripts. Usually located in distinct files in system. Either come pre-installed with the distribution package manager, or can be created or installed by the user. 

    mv, date, uptime, cp ...

<br>

    $ type echo
    echo is a shell builtin

    $ type uptime
    uptime is hashed (/usr/bin/uptime)

<br>

- `pushd`: This command remembers the current working directory before changing to the directory specified in the command argument.
- `popd`:

<br>

- `man`: - an interface to the system reference manuals
- `whatis`: - display one-line manual page descriptions
  - `command --help`
  - `apropos command`: search the manual page names and descriptions
- `whoami`: returns username.
- `id`: - print real and effective user and group IDs
- `ssh`:  OpenSSH remote login client
  - `ssh username@xxx.xx.xx.x`
    - example: `ssh applepie@172.16.238.3`

- `alias`: user-defined command for existing shell commands.

      alias dt=date
      dt
      Fri 21 Jan 2022 04:46:02 PM PST

- `history`

- `ls -l`: long list
- `ls -a`: list all files including hidden
- `ls -lt`: long list files in order created
- `ls -ltr`: longlist files in the reverse order created
- `ls /etc/*release*`: check OS version.
- `cat /etc/*release*`: check OS version details.

<br>

**Environmental variable** 

- `$UPPERCASE`: represents environmental variable. 

- `echo $HOME`: prints home directory.
- `echo $SHELL`: prints the shell you are using. 
- `echo -n`: print without a new trailing line.

Create an environment variable

- `export VARIALBE=VALUE`

      export COURSE=DEVOPS
      echo 'export COURSE=DEVOPS' >> /home/CodingForest/.profile

- `cat ~/.profile`
- `~/.pam_environment`

<br>

**Path variable**

- `echo $PATH`: the `paths` your machine is looking for commands.
- `export PATH=$PATH:[path]`: add [path] to PATH variable

Example 1

      export PATH=$PATH:/opt/obs/bin

      which obs-studio
      > /opt/obs/bin/obs-studio

<br>

Example 2

      PS1='[\d]\u@\h:\w$'
      echo 'PS1="[\d]\u@\h:\w$"' >> ~/.profile


<br>

**Special shell environment variable**

- `echo $PS1`: change this variable to custom-format the bash prompt. 

      PS1="[\d \t \u@\h:\w ] $"
      [Fri Jan 21 17:06:47 CodingForest@ubuntu:~ ] $

<br>

- `uptime`: Tell how long the system has been running.

      11:51:38 up  3:20,  1 user,  load average: 1.03, 0.92, 0.90

- `apt`: Advanced Packaging Tool. a powerful command-line interface, which works with Ubuntu's Advanced Packaging Tool (APT) performing such functions as installation of new software packages, upgrade of existing software packages, updating of the package list index, and even upgrading the entire Ubuntu system.
- `touch`: creates a file without any content
- `mkdir -p`: creates a directory. `-p` stands for parent. Creates parent directories as necessary.
- `rm -r`: will _**recursively**_ delete a directory and all its contents (normally rm will not delete directories, while rmdir will only delete empty directories).
- `freeze`: `pip3 freeze` lists all python libraries installed on your machine. 
- `which`: locate a command.
- `type`: `type python3`
  - `python3 is hashed (/usr/bin/python3)`: Python is running because it is `in path`.
- `-xvf`: short method to implement –extract –verbose –file. So, x stands for extracting the archive, v for displaying Verbose information, and f for specifying a filename.

        tar -xvf openjdk-17.0.2_linux-aarch64_bin.tar.gz


- `cd /path/; tree`

        cd /opt/app/WeatherApp/; tree

        .
        ├── pom.xml
        └── src
            ├── main
            │   └── java
            │       └── org
            │           └── ForestFoundation
            │               └── app
            │                   └── WeatherApp.java
            └── test
                └── java
                    └── org
                        └── ForestFoundation
                            └── app
                                └── WeatherAppTest.java


- `curl https://url -O`: Client URL. `-O` tag saves the web content in the form of a file.
- `wget https://url -O file.txt`: Optionally, you can specify the file name.



## <span id="setup">Setup</span>

**Install Ubuntu**

- Donwload Ubuntu `ISO`   
- Install Ubuntu `ISO` on VMWare   

https://ubuntu.com/download/desktop

<br>

## <span id='docker'>🐳Docker</span>

**Install 🐳Docker**

    sudo apt-get update 
    sudo apt-get install ca-certificates curl gnupg lsb-release

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

<br>

**Check result**

    sudo docker version

<br>

## <span id='github'> Github </span>

**Install GitHub**

`sudo dpkg -i GitHubDesktop-linux-2.9.3-linux3.deb` << Install   
`sudo apt update && sudo apt install -f`  << Fix dependency error   
`sudo dpkg -i GitHubDesktop-linux-2.9.3-linux3.deb` << Re-install after the fix.    

https://meshworld.in/install-github-desktop-on-ubuntu-20-04-or-ubuntu-based-distributions/  

<br>

## <span id='pip'>`pip`</span>

**Install `pip`**
sudo apt install python3-pip

<br>

## <span id='vscode'>VS Code</span>

**Install VS Code**

`sudo dpkg -i code_1.63.2-1639562499_amd64.deb`

https://code.visualstudio.com/download

<br>

## <span id='mouse'>Mouse</span>

**Install mouse**

Mode: Basilisk Essential

General

    sudo gpasswd -a $USER plugdev

Ubuntu 

    sudo apt install software-properties-gtk
    sudo add-apt-repository ppa:openrazer/stable
    sudo add-apt-repository ppa:openrazer/daily
    sudo apt update
    sudo apt install openrazer-meta


https://openrazer.github.io/#download

<br>

**Mouse button setup**

    xinput list
    xinput set-prop 10 "libinput Middle Emulation Enabled" 1
    xinput list-props 10

https://askubuntu.com/questions/160164/how-do-i-enable-middle-mouse-button-emulation-in-12-04-lts/902976#902976

<br>

**Mouse side button**

https://askubuntu.com/questions/1022814/mouse-wheel-side-buttons-not-working

https://github.com/cdobrich/btnx-config

<br>

**Install Pinta**

`sudo apt update`
`sudo apt install snapd`
`sudo snap install pinta`

https://snapcraft.io/install/pinta/ubuntu


<br>
<br>

### References

  - https://kernel.org
  - https://www.asciiart.eu/
 