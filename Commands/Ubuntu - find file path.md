## Context
    
Where is my `jdk` file in my computer?
    
<br>

## Command

Short version:

    which java 

    /usr/bin/java

Long version:    

    readlink -f $(which java) 

    /usr/lib/jvm/java-11-openjdk-amd64/bin/java

<br>

### References

https://stackoverflow.com/questions/16931327/where-is-the-java-sdk-folder-in-my-computer-ubuntu-12-04