# Networking

<br> 

**Jump Host**

A jump server, jump host or jump box is a system on a network used to access and manage devices in a separate security zone. A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them. The most common example is managing a host in a DMZ from trusted networks or computers.


## `Switching`: Inter-system networking setup

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2001.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2009.png" width=1000 />


| system 1   |  system 2  |
| -- | -- |
| `ip link`   | `ip link`   |
| `ip addr add 192.168.1.10/24 dev eth0`   |  `ip addr add 192.168.1.11/24 dev eth0`  |
| `ping 192.168.1.11`   |    |

<br>

## `Routing` and `Gateway`: Inter-network networking setup

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2002.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2003.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2004.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2005.png" width=1000 />

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

- A gateway can connect a system to the internet.

<br>

## Commands

      ip route add 192.168.2.0/24 via 192.168.1.1
      
      route
      Destination       Gateway           Genmask           Iface
      192.168.2.0       192.168.1.1       255.255.255.0     eth0

If you want to assign an IP address to a system, a system must have a physical or virtual network interface attached to it. There are other interfaces such as `eth`, `veth`, and `lo`.

      ip link 

      1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
         link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      19: eth0@if20: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
         link/ether 02:42:ac:10:ee:0a brd ff:ff:ff:ff:ff:ff link-netnsid 0
      23: eth1@if24: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default 
         link/ether 02:42:ac:11:00:05 brd ff:ff:ff:ff:ff:ff link-netnsid 0

<br>

- `ip route`

      default via 172.16.238.1 dev eth0 
      172.16.238.0/24 dev eth0 proto kernel scope link src 172.16.238.10 
      172.17.0.0/16 dev eth1 proto kernel scope link src 172.17.0.5 

- `route`: displays IP route table.

      Kernel IP routing table
      Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
      default         gateway         0.0.0.0         UG    0      0        0 eth0
      172.16.238.0    0.0.0.0         255.255.255.0   U     0      0        0 eth0
      172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth1

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2006.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2007.png" width=1000 />

<br>
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2010.png" width=1000 />


## <span id="DNS">DNS</span>

[[☝️top]](#top)

- `DNS`: resolves IP of a domain name. 
- `ping`: a computer network administration software utility used to test the reachability of a host on an Internet Protocol (IP) network.
- `/etc/hosts`: points domains/hostnames to IPs locally.
- `/etc/resolv.conf`: contains information about dns server (nameserver).

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20011.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20012.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20013.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20014.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20015.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20016.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20017.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20018.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%20019.png" width=1000 />
<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2020.png" width=1000 />


<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2023.png" width=1000 />

To edit `/etc/hosts`, you can do, for example: 

      sudo vi /etc/hosts

      127.0.0.1       localhost
      ::1     localhost ip6-localhost ip6-loopback
      fe00::0 ip6-localnet
      ff00::0 ip6-mcastprefix
      ff02::1 ip6-allnodes
      ff02::2 ip6-allrouters
      172.16.xxx.x    imaginary_website.com host01
      172.17.x.x      imaginary_website.com host01
      127.0.0.1       www.google.com host01

## Search Domain 

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2022.png" width=1000 />

If you want to omit the default domain name when searching on address bar, 
you can have the `/etc/resolv.conf` append the default domain name by adding the following line - `search [default_domian_name]`:

      [/etc/resolv.conf]

      search stratos.xfusioncorp.com
      nameserver 127.0.0.11
      nameserver 8.8.8.8
      search google.com
      options ndots:0

Now if you search `drive`, for instance, the browser will direct you to `drive.google.com`.

## NSLoopup

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Linux%2021.png" width=1000 />

- `dig`: is a network administration command-line tool for querying the Domain Name System (DNS). dig is useful for network troubleshooting and for educational purposes. 

<br>