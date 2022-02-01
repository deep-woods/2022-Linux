## Issue
    
    cat /etc/resolv.conf
    nameserver 8.8.8.8 

    ping 8.8.8.8
    connect: Network is unreachable

<br>

## Reason

Network is unreachable. This means that your natowrk is not active at the moment.

<br>

## Solution

_It seems from your 192.168.56.* address that you are running in a VirtualBox image, and that you are wondering why the image has no connectivity when presumably your host does._

_You have probably set the first eth0 interface to "NAT" or "bridged", and attached it to your host's ethernet interface. If you are online via wifi but your bridge/NAT is configured to use the ethernet card, you won't have network connectivity; the reverse is also true._

_Change the network interface of the VM to use the correct host interface._ (by taifwa on StackExchange)
    

<br>

### References 

- https://askubuntu.com/questions/900556/ping-8-8-8-8-connect-network-is-unreachable