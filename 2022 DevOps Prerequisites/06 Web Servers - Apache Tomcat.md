# 06 Web Servers - Apache Tomcat

<br>

- The Apache Tomcat server provides a web server environment 
  - where we can host `Java`-based web applications.
    
<br>

### Setup 

Install

- Install `java`
- Install `Apache Tomcat` installation file from the Apache Tomcat downloads page https://tomcat.apache.org/whichversion.html

<br>

**Ubuntu**

    java --version
    openjdk 11.0.13 2021-10-19
    OpenJDK Runtime Environment (build 11.0.13+8-Ubuntu-0ubuntu1.20.04)
    OpenJDK 64-Bit Server VM (build 11.0.13+8-Ubuntu-0ubuntu1.20.04, mixed mode, sharing)


<br>

**CentOS**

    sudo yum install java-1.8.0-openjdk-devel

<br>

Use either `curl` or `wget`. Both work. 

    sudo curl -OL https://downloads.apache.org/tomcat/tomcat-8/v8.5.75/bin/apache-tomcat-8.5.75.tar.gz
    sudo wget https://downloads.apache.org/tomcat/tomcat-8/v8.5.75/bin/apache-tomcat-8.5.75.tar.gz

    --2022-01-24 14:17:37--  https://downloads.apache.org/tomcat/tomcat-8/v8.5.75/bin/apache-tomcat-8.5.75.tar.gz
    Resolving downloads.apache.org (downloads.apache.org)... 135.181.214.104, 88.99.95.219, 2a01:4f8:10a:201a::2, ...
    Connecting to downloads.apache.org (downloads.apache.org)|135.181.214.104|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 10595855 (10M) [application/x-gzip]
    Saving to: ‘apache-tomcat-8.5.75.tar.gz’

    sudo tar xvf apache-tomcat-8.5.75.tar.gz

    sudo mv apache-tomcat-8.5.75 /opt/apache-tomcat-8 

<br>

start `apache tomcat` server.

    ./apache-tomcat-8.5.53/bin/startup.sh

    Using CATALINA_BASE:   /home/forest/apache-tomcat-8.5.75
    Using CATALINA_HOME:   /home/forest/apache-tomcat-8.5.75
    Using CATALINA_TMPDIR: /home/forest/apache-tomcat-8.5.75/temp
    Using JRE_HOME:        /usr
    Using CLASSPATH:       /home/forest/apache-tomcat-8.5.75/bin/bootstrap.jar:/home/forest/apache-tomcat-8.5.75/bin/tomcat-juli.jar
    Using CATALINA_OPTS:   
    Tomcat started.


<br>

In production environment:

- Configure users specific to Tomcat
- Configure Tomcat as a service.

<br>

# Apache Tomcat Directory Structure

    ls -l /opt/apache-tomcat-8/apache-tomcat-8.5.75

    total 148
    drwxr-x--- 2 forest forest  4096 Jan 24 14:18 bin
    drwxr-x--- 2 forest forest  4096 Jan 24 14:18 lib
    drwx------ 2 forest forest  4096 Jan 17 22:07 conf
    drwxr-x--- 2 forest forest  4096 Jan 17 22:07 logs
    drwxr-x--- 2 forest forest  4096 Jan 24 14:18 temp
    drwxr-x--- 7 forest forest  4096 Jan 17 22:07 webapps
    drwxr-x--- 2 forest forest  4096 Jan 17 22:07 work
    -rw-r----- 1 forest forest 19481 Jan 17 22:07 BUILDING.txt
    -rw-r----- 1 forest forest  6210 Jan 17 22:07 CONTRIBUTING.md
    -rw-r----- 1 forest forest 57011 Jan 17 22:07 LICENSE
    -rw-r----- 1 forest forest  1726 Jan 17 22:07 NOTICE
    -rw-r----- 1 forest forest  3378 Jan 17 22:07 README.md
    -rw-r----- 1 forest forest  7136 Jan 17 22:07 RELEASE-NOTES
    -rw-r----- 1 forest forest 16507 Jan 17 22:07 RUNNING.txt

<br>

## `bin`

- `startup.sh`, `shutdown.sh`: start and stop the server in `linux`
- `startup.bat`, `shutdown.bat`: start and stop the server in `windows`

    ls -l /opt/apache-tomcat-8/bin

    total 876
    -rw-r----- 1 forest forest  36431 Jan 17 22:07 bootstrap.jar
    -rw-r----- 1 forest forest  16840 Jan 17 22:07 catalina.bat
    -rwxr-x--- 1 forest forest  25294 Jan 17 22:07 catalina.sh
    -rw-r----- 1 forest forest   1664 Jan 17 22:07 catalina-tasks.xml
    -rw-r----- 1 forest forest   2123 Jan 17 22:07 ciphers.bat
    -rwxr-x--- 1 forest forest   1997 Jan 17 22:07 ciphers.sh
    -rw-r----- 1 forest forest  25357 Jan 17 22:07 commons-daemon.jar
    -rw-r----- 1 forest forest 207420 Jan 17 22:07 commons-daemon-native.tar.gz
    -rw-r----- 1 forest forest   2040 Jan 17 22:07 configtest.bat
    -rwxr-x--- 1 forest forest   1922 Jan 17 22:07 configtest.sh
    -rwxr-x--- 1 forest forest   9100 Jan 17 22:07 daemon.sh
    -rw-r----- 1 forest forest   2091 Jan 17 22:07 digest.bat
    -rwxr-x--- 1 forest forest   1965 Jan 17 22:07 digest.sh
    -rw-r----- 1 forest forest   3460 Jan 17 22:07 setclasspath.bat
    -rwxr-x--- 1 forest forest   3708 Jan 17 22:07 setclasspath.sh
    -rw-r----- 1 forest forest   2020 Jan 17 22:07 shutdown.bat
    -rwxr-x--- 1 forest forest   1902 Jan 17 22:07 shutdown.sh
    -rw-r----- 1 forest forest   2022 Jan 17 22:07 startup.bat
    -rwxr-x--- 1 forest forest   1904 Jan 17 22:07 startup.sh
    -rw-r----- 1 forest forest  51708 Jan 17 22:07 tomcat-juli.jar
    -rw-r----- 1 forest forest 428057 Jan 17 22:07 tomcat-native.tar.gz
    -rw-r----- 1 forest forest   4574 Jan 17 22:07 tool-wrapper.bat
    -rwxr-x--- 1 forest forest   5540 Jan 17 22:07 tool-wrapper.sh
    -rw-r----- 1 forest forest   2026 Jan 17 22:07 version.bat
    -rwxr-x--- 1 forest forest   1908 Jan 17 22:07 version.sh

<br>

## `conf`

In this directory you can configure: 

- which port to listen
- direct traffic between the different web apps

<br>

`server.xml`: this file has an entry for connector. The connector is the endpoint at which the requests are received.
  - `8080`: the default port that Tomcat listens on. You can change it if need be. 
  - Any changes to `server.xml` will require a restart of the Tomcat service.

<br>

`dot.xml`: deploy and configure web applications.


<br>

    ls -l /opt/apache-tomcat-8/conf

    total 224
    -rw------- 1 forest forest  12954 Jan 17 22:07 catalina.policy
    -rw------- 1 forest forest   7675 Jan 17 22:07 catalina.properties
    -rw------- 1 forest forest   1338 Jan 17 22:07 context.xml
    -rw------- 1 forest forest   1149 Jan 17 22:07 jaspic-providers.xml
    -rw------- 1 forest forest   2313 Jan 17 22:07 jaspic-providers.xsd
    -rw------- 1 forest forest   3916 Jan 17 22:07 logging.properties
    -rw------- 1 forest forest   7580 Jan 17 22:07 server.xml
    -rw------- 1 forest forest   2756 Jan 17 22:07 tomcat-users.xml
    -rw------- 1 forest forest   2558 Jan 17 22:07 tomcat-users.xsd
    -rw------- 1 forest forest 171879 Jan 17 22:07 web.xml

<br>

## `logs`

To see logs and errors:

    ls -l /opt/apache-tomcat-8/logs

<br>

## `webapps`

Web applications hosted by Tomcat server reside here. This is where you should place your applications to serve via Apache Tomcat.

    ls -l /opt/apache-tomcat-8/webapps

    total 20
    drwxr-x--- 15 forest forest 4096 Jan 24 14:18 docs
    drwxr-x---  7 forest forest 4096 Jan 24 14:18 examples
    drwxr-x---  6 forest forest 4096 Jan 24 14:18 host-manager
    drwxr-x---  6 forest forest 4096 Jan 24 14:18 manager
    drwxr-x---  3 forest forest 4096 Jan 24 14:18 ROOT

<br>

## `.war`

- War stands for Web Archive. Although Apache is for `Java`, you have to package your web applications using `WAR` instead of `JAR` because web applications include `non-java` code like `HTML`, `CSS`, etc.
- Use build tools like Maven or Gradle to build and package the application.
        
        jar -cvf app.war *
        mvn package
        gradle build

        Then move the `app.war` into `webapps`.

- If the `tomcat` server is already running, it will automatically detect if an application is placed in the `webapps` directory. The server will extract the `war` file into a directory in its name.
- Then the application is hosted at a path by the name of the extracted directory (`war` file name).

<br>
<br>

## Exercise 

Start the server. 

### CentOs


`sudo yum install -y wget`  
`wget https://downloads.apache.org/tomcat/tomcat-8/v8.5.75/bin/apache-tomcat-8.5.75.tar.gz`  
`tar xvf apache-tomcat-8.5.75.tar.gz`  
`sudo mv apache-tomcat-8.5.75 /opt/apache-tomcat-8`  
`sudo /opt/apache-tomcat-8/bin/startup.sh`  

    Using CATALINA_BASE:   /opt/apache-tomcat-8/apache-tomcat-8.5.75
    Using CATALINA_HOME:   /opt/apache-tomcat-8/apache-tomcat-8.5.75
    Using CATALINA_TMPDIR: /opt/apache-tomcat-8/temp
    Using JRE_HOME:        /usr
    Using CLASSPATH:       /opt/apache-tomcat-8/bin/bootstrap.jar:/opt/apache-tomcat-8/bin/tomcat-juli.jar
    Using CATALINA_OPTS:   
    Tomcat started.

<br>

`curl localhost:8081; ps -ef | grep tomcat`
  
    curl: (7) Failed to connect to ::1: Cannot assign requested address
    root       405     1  0 14:49 pts/0    00:00:04 /usr/bin/java -Djava.util.logging.config.file=/opt/apache-tomcat-8/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -classpath /opt/apache-tomcat-8/bin/bootstrap.jar:/opt/apache-tomcat-8/bintomcat-juli.jar -Dcatalina.base=/opt/apache-tomcat-8/apache-tomcat-8.5.75 -Dcatalina.home=/opt/apache-tomcat-8/apache-tomcat-8.5.75 -Djava.io.tmpdir=/opt/apache-tomcat-8/temp org.apache.catalina.startup.Bootstrap start
    forest       462    41  0 14:51 pts/0    00:00:00 grep --color=auto tomcat

<br>

The server is running, but the port configuration is wrong. You cannot access the page.  
Fix the port configuration to make the service up and running again. 

- 1. Change port number
  - `sed`: Stream editor for filtering and transforming text https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/
  - `sed OPTIONS... [SCRIPT] [INPUTFILE...]`
  - `/g`: (global replacement) 
  - `sudo sed -i 's/8081/9090/g' /opt/apache-tomcat-8/conf/server.xml;`

    <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL/TLS HTTP/1.1 Connector on port 8081
    -->
    <Connector port="8081" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />

<br>

- 2. Shutdown and restart the `tomcat` server.
  - `sudo /opt/apache-tomcat-8/bin/shutdown.sh`

        Using CATALINA_BASE:   /opt/apache-tomcat-8
        Using CATALINA_HOME:   /opt/apache-tomcat-8
        Using CATALINA_TMPDIR: /opt/apache-tomcat-8/temp
        Using JRE_HOME:        /usr
        Using CLASSPATH:       /opt/apache-tomcat-8/bin/bootstrap.jar:/opt/apache-tomcat-8/bin/tomcat-juli.jar
        Using CATALINA_OPTS:   

  - `sudo /opt/apache-tomcat-9/bin/startup/sh`

        Using CATALINA_BASE:   /opt/apache-tomcat-8
        Using CATALINA_HOME:   /opt/apache-tomcat-8
        Using CATALINA_TMPDIR: /opt/apache-tomcat-8/temp
        Using JRE_HOME:        /usr
        Using CLASSPATH:       /opt/apache-tomcat-8/bin/bootstrap.jar:/opt/apache-tomcat-8/bin/tomcat-juli.jar
        Using CATALINA_OPTS:   
        Tomcat started.

<br>

- 3. Additionally.. if you want to double check the `tomcat` page content and process: 
  - curl localhost:9090; ps -ef | grep tomcat`

        root       628     1  0 15:30 pts/1    00:00:05 /usr/bin/java -Djava.util.logging.config.file=/opt/apache-tomcat-8/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -classpath /opt/apache-tomcat-8/bin/bootstrap.jar:/opt/apache-tomcat-8/bin/tomcat-juli.jar -Dcatalina.base=/opt/apache-tomcat-8 -Dcatalina.home=/opt/apache-tomcat-8 -Djava.io.tmpdir=/opt/apache-tomcat-8/temp org.apache.catalina.startup.Bootstrap start
        thor       699   108  0 15:30 pts/1    00:00:00 grep --color=auto tomcat
<br>


Checking logs. `catalina.out` shows the logs for extracting war content into tomcat webapps directory

- `sudo sh -c 'grep sample.war /opt/apache-tomcat-8/logs/*'`