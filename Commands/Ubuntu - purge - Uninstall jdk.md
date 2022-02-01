## Context
    
- Uninstall unwanted `jdk` from my machine.
- I want to remove `11`.

        sudo dpkg --list | grep -i jdk

        ii  openjdk-11-jre:amd64                       11.0.13+8-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT
        ii  openjdk-11-jre-headless:amd64              11.0.13+8-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT (headless)
        ii  openjdk-8-jre:amd64                        8u312-b07-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT
        ii  openjdk-8-jre-headless:amd64               8u312-b07-0ubuntu1~20.04              amd64        OpenJDK 

<br>

## Command

    sudo apt purge openjdek-11-jre:amd64

 
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following packages will be REMOVED
        default-jre-headless* openjdk-11-jre-headless*
    0 to upgrade, 0 to newly install, 2 to remove and 17 not to upgrade.
    After this operation, 171 MB disk space will be freed.

    Removing default-jre (2:1.11-72) ...
    Removing openjdk-11-jre:amd64 (11.0.13+8-0ubuntu1~20.04) ...

Repeat the process for every version you want to remove. 

    sudo apt purge openjdk-11-jre-headless:amd64

    Removing default-jre-headless (2:1.11-72) ...
    Removing openjdk-11-jre-headless:amd64 (11.0.13+8-0ubuntu1~20.04) ...

    Purging configuration files for openjdk-11-jre-headless:amd64 (11.0.13+8-0ubuntu1~20.04) ...


<br>

### References

- https://www.youtube.com/watch?v=OL0a45h0vxQ