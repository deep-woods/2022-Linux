## Context

VMware stopped running because of memory shortage. So I try to shut it down, but then it refuses to power off. 
    
<br>

## Command

    $ ps -ef | grep vmw          <------------- shows list of processes with process ID.
    $ kill -9 <ps ID>

<br>

### References

https://www.linuxsecrets.com/2318-how-to-kill-a-locked-vmware-session
