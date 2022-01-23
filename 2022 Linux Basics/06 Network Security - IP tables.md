# Network Security - IP tables

## SSH connection 

**requirements**

- Username & password
- SSH keys
- SSH/port22

<br>

**Security measures**

+ Use IP table to filter network traffic within Linux systems
+ External firewalls: FirewallD (Linux), Firewall (Windows)

<br>

|  User  |  Switches  |  Web server  |  DB server  |
| -- | -- | -- | -- |
|  client/laptop |   **external firewalls or appliances**<br>Cisco ASA<br>Juniper next-gen firewall<br>Barracuda next-gen firewall<br>Fortinet  |  local IP table for host1  |    |
|  client/laptop  |    |  local  IP table for host2  |    |
|  client/laptop  |    |  local  IP table for host3  |    |

<br>


### Setup 

- `sudo apt install iptables`

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    iptables is already the newest version (1.8.4-3ubuntu2).
    iptables set to manually installed.
    0 upgraded, 0 newly installed, 0 to remove and 77 not  upgraded.

<br>

- `sudo iptables -L`: list default rules configured in the system

      Chain INPUT (policy ACCEPT)
      target     prot opt source               destination         

      Chain FORWARD (policy DROP)
      target     prot opt source               destination         
      DOCKER-USER  all  --  anywhere             anywhere            
      DOCKER-ISOLATION-STAGE-1  all  --  anywhere             anywhere            
      ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
      DOCKER     all  --  anywhere             anywhere            

      Chain OUTPUT (policy ACCEPT)
      target     prot opt source               destination         

<br>

- `INPUT`: applicable to network traffic coming into the system.
  - example: to allow the SSH connection from the client laptop
- `OUTPUT`: connections initiated by the host server to other systems
  - host server queries a DB server or writes data.
- `FORWARD`: data is forwarded to other devices in the network
- `(policy ACCEPT)`: by default, the rule is set to accept for each connection type. 

<br>

- It is also possible to  configure the condition to consider the destination where the traffic is headed to, the port at which the traffic as well as the protocol.

<br>

**IP Table example rules**

Clients 1, 2, and 4 are accepted, everything else will be dropped. 

|  From<br>client 1  |    |    |    |    |
| -- | -- | -- | -- | -- |
|    |  From<br>client 2   |    |    |    |
|    |    |    |    |  From<br>client 5   |
|  Accept<br>if<br>src is client 01  | Drop<br>if<br>src is client 02   | Accept<br>if<br>src is client 03   |   Accept<br>if<br>src is client 04 |  Drop<br>if<br>src is client 05  |

<br>
<br>

## Securing the Environments

<br>

- `iptables -A INPUT -p tcp -s <IP Address> --dport 22 -j ACCEPT`: n

**`iptables` command flags**

| Option   | Description   |
| -- | -- |
|  -A  |  Add rule  |
|  -I  |  Insert rule<br>(at the top of the chain) |
|  -p  |  Protocol  |
|  -s  |  Source  |
|  -d  |  Destination  |
|  --dport  |  Destination port  |
|  -j  | Action to take  |

<br>

**Add INPUT rules**

|    |    |
| -- | -- |
| -A   | INPUT<br>OUTPUT<br>FORWARD   |
| -p  |  TCP<br>UDP   |
| --dport  | SSH/port22<br>HTTP/port80<br>HTTPS/port443   |
|  -j  |  ACCEPT<br>DROP  |

<br>

- `iptables -A INPUT -p tcp -s <IP Address> --dport 22 -j ACCEPT`:
- Below IP table rules permit both `SSH` and `HTTP` connections from `172.xx.xxx.xxx`.

    iptables -A INPUT -p tcp -s 172.xx.xxx.xxx --dport 22 -j ACCEPT  (rule 1)
    iptables -A INPUT -p TCP -s 172.xx.xxx.xxx --dport 80 -j ACCEPT  (rule 2)
    
    iptables -L

    forest@forestApp01:~$ sudo iptables -L
    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination         
    ACCEPT     tcp  --  172.xx.xxx.xxx       anywhere             tcp dpt:ssh   1
    ACCEPT     tcp  --  172.xx.xxx.xxx       anywhere             tcp dpt:http  2

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination         

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination         

<br>
<br>

### IP table configuration

**Example 1**

<br>

|  Source/dest  |  Action  |
| -- | -- |
|  forest-db-01<br>`172.00.00.10`  |  Allow outgoing connection to port 5432<br>`iptables -A OUTPUT -p tcp -d 172.00.00.20 --dport 5432 -j ACCEPT` |
|  forest-repo-01<br>`172.00.00.20`  |  Allow outgoing connection to port 80<br>`iptables -A OUTPUT -p tcp -d 172.00.00.10 --dport 80 -j ACCEPT`  |
|  forest-walker-01<br>`172.00.00.30`  |  Allow incoming on port 80<br>`iptables -A INPUT -p tcp -s 172.00.00.20 --dport 80 -j ACCEPT`  |
|  forest-app-01<br>`172.00.00.40`  |  Lockdown all incoming traffic<br>`iptables -A INPUT -j DROP`<br>Allow access to port 5432 on dev-db01, and HTTP access to forest-repo01.<br>`iptables -A OUTPUT -d 172.00.00.10 --dport 5432 -j ACCEPT`<br>`iptables -A -D 172.00.00.02 --dport 80 -j ACCEPT`<br>Block outgoing traffic to any on `http/https` ports from forest-app-01.<br>`` |
|  internet<br>`172.00.00.80`  |  Drop all outgoing connections port 80/443 (HTTP/HTTPS)<br>`iptables -A OUTPUT -p tcp --dport 80 -j DROP`<br>`iptables -A OUTPUT -p tcp --dport 443 -j DROP`  |

<br>

**Example 2**

- `sudo iptables -A INPUT -p tcp -s 172.00.00.20 --dport 22 -j ACCEPT`:
- `sudo iptables -A INPUT -p tcp -s 172.00.00.20 --dport 80 -j ACCEPT`:

      sudo iptables -L

      Chain INPUT (policy ACCEPT)
      target     prot opt source                destination         
      ACCEPT     tcp  --  forest-repo-10        anywhere             tcp dpt:ssh
      ACCEPT     tcp  --  forest-repo-10        anywhere             tcp dpt:http

<br>

- `udo iptables -A INPUT -p tcp -j DROP`
- `udo iptables -A INPUT -p udp -j DROP`   

Alternatively-  
- `udo iptables -A INPUT -j DROP`: do not specify `protocol`, then the rule will drop both `tcp` & `udp` connections.

      Chain INPUT (policy ACCEPT)
      target     prot opt source               destination         
      ACCEPT     tcp  --  forest-repo-10       anywhere             tcp dpt:ssh
      ACCEPT     tcp  --  forest-repo-10       anywhere             tcp dpt:http
      DROP       all  --  anywhere             anywhere            

<br>

- `sudo iptables -A OUTPUT -p tcp -d 172.00.00.10 --dport 5432 -j ACCEPT`
- `sudo iptables -A OUTPUT -p tcp -d 172.00.00.20 --dport 80 -j ACCEPT`
- `sudo iptables -A OUTPUT -p tcp --dport 80 -j DROP`
- `sudo iptables -A OUTPUT -p tcp --dport 443 -j DROP`

      Chain OUTPUT (policy ACCEPT)
      target     prot opt source               destination         
      ACCEPT     tcp  --  anywhere             forest-db-01         tcp dpt:postgresql
      ACCEPT     tcp  --  anywhere             forest-repo-01       tcp dpt:http
      DROP       tcp  --  anywhere             anywhere             tcp dpt:http
      DROP       tcp  --  anywhere             anywhere             tcp dpt:https

<br>

- `sudo iptables -I OUTPUT -p tcp -d google.com --dport 443 -j ACCEPT`: inject to the top of the list.

      Chain OUTPUT (policy ACCEPT)
      target     prot opt source               destination         
      ACCEPT     tcp  --  anywhere             google.com           tcp dpt:https
      ACCEPT     tcp  --  anywhere             forest-db-01         tcp dpt:postgresql
      ACCEPT     tcp  --  anywhere             forest-repo-01       tcp dpt:http
      DROP       tcp  --  anywhere             anywhere             tcp dpt:http
      DROP       tcp  --  anywhere             anywhere             tcp dpt:https
