## Context
    
What are `jdk`s installed on my machine?
    
<br>

## Command

    sudo dpkg --list | grep -i jdk

This command lists all the `jdks` currently available in the machine. I have two: `8` and `11`.

    ii  openjdk-11-jre:amd64                       11.0.13+8-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT
    ii  openjdk-11-jre-headless:amd64              11.0.13+8-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT (headless)
    ii  openjdk-8-jre:amd64                        8u312-b07-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT
    ii  openjdk-8-jre-headless:amd64               8u312-b07-0ubuntu1~20.04              amd64        OpenJDK Java runtime, using Hotspot JIT (headless)

<br>

### References

- https://www.youtube.com/watch?v=OL0a45h0vxQ