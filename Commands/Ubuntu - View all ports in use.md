## Context
    
There are conflicts in port assignment. I need to find free ports. 

<br>

## Command

    less /etc/services

See what you get...!

    # Network services, Internet style
    #
    # Note that it is presently the policy of IANA to assign a single well-known
    # port number for both TCP and UDP; hence, officially ports have two entries
    # even if the protocol doesn't support UDP operations.
    #
    # Updated from http://www.iana.org/assignments/port-numbers and other
    # sources like http://www.freebsd.org/cgi/cvsweb.cgi/src/etc/services .
    # New ports will be added on request if they have been officially assigned
    # by IANA and used in the real-world or are needed by a debian package.
    # If you need a huge list of used numbers please install the nmap package.

    tcpmux          1/tcp                           # TCP port service multiplexer
    echo            7/tcp
    echo            7/udp
    discard         9/tcp           sink null
    discard         9/udp           sink null
    systat          11/tcp          users
    daytime         13/tcp
    daytime         13/udp
    netstat         15/tcp
    qotd            17/tcp          quote
    msp             18/tcp                          # message send protocol
    msp             18/udp
    chargen         19/tcp          ttytst source
    chargen         19/udp          ttytst source
    ftp-data        20/tcp
    ftp             21/tcp
    fsp             21/udp          fspd
    ssh             22/tcp                          # SSH Remote Login Protocol

<br>

### References

- Vivek Gite (2022) How to check if port is in use on Linux or Unix https://www.cyberciti.biz/faq/unix-linux-check-if-port-is-in-use-command/
