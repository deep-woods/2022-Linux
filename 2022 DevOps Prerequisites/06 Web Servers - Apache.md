# 06 Web Servers - Apache Web Server

<br>

- Web server hosting web content (CSS, HTML, JavaScript files)
- Open source
    
<br>

### Setup 

Install

`apt install apache2` (Ubuntu)  
`sudo service apache2 start`
`sudo service apache2 status`

`yum install httpd` (CentOS)  
`sudo service httpd start`
`sudo service httpd status`


<br>

### Ubuntu

    sudo service apache2 start
    sudo service apache2 status

    ● apache2.service - The Apache HTTP Server
        Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor prese>
        Active: active (running) since Mon 2022-01-24 21:43:16 KST; 4min 44s ago
        Docs: https://httpd.apache.org/docs/2.4/
    Main PID: 27878 (apache2)
        Tasks: 55 (limit: 4593)
        Memory: 4.7M
        CGroup: /system.slice/apache2.service
                ├─27878 /usr/sbin/apache2 -k start
                ├─27879 /usr/sbin/apache2 -k start
                └─27880 /usr/sbin/apache2 -k start

    Jan 24 21:43:16 ubuntu systemd[1]: Starting The Apache HTTP Server...
    Jan 24 21:43:16 ubuntu apachectl[27877]: AH00558: apache2: Could not reliably d>
    Jan 24 21:43:16 ubuntu systemd[1]: Started The Apache HTTP Server.

<br>

If a firewall is configured on your system, then add a rule to allow HTTP traffic.

    sudo apt install firewalld


<br>

### CentOS

    sudo service httpd start

    Redirecting to /bin/systemctl start httpd.service
    
<br>

    sudo service httpd status
    
    Redirecting to /bin/systemctl status httpd.service
    ● httpd.service - The Apache HTTP Server
        Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
        Active: active (running) since Mon 2022-01-24 12:54:41 UTC; 8s ago
            Docs: man:httpd(8)
                man:apachectl(8)
        Main PID: 1190 (httpd)
        Status: "Processing requests..."
        CGroup: /docker/586878b32293e7f74bfe852f42032f5d0370396c66196f7f2a5dcef7d7db6390/system.slice/httpd.service
                ├─1190 /usr/sbin/httpd -DFOREGROUND
                ├─1191 /usr/sbin/httpd -DFOREGROUND
                ├─1192 /usr/sbin/httpd -DFOREGROUND
                ├─1193 /usr/sbin/httpd -DFOREGROUND
                ├─1194 /usr/sbin/httpd -DFOREGROUND
                └─1195 /usr/sbin/httpd -DFOREGROUND


    firewall-cmd --permanent --

<br>

### Logs

To see logs and errors:

For log

    sudo cat /var/log/httpd/access_log

For error: `ErrorLog "logs/error_log"`

    sudo cat /var/log/httpd/error_log

    [Mon Jan 24 12:54:41.103224 2022] [suexec:notice] [pid 1190] AH01232: suEXEC mechanism enabled (wrapper: /usr/sbin/suexec)
    [Mon Jan 24 12:54:41.114100 2022] [lbmethod_heartbeat:notice] [pid 1190] AH02282: No slotmem from mod_heartmonitor
    [Mon Jan 24 12:54:41.118279 2022] [mpm_prefork:notice] [pid 1190] AH00163: Apache/2.4.6 (CentOS) configured -- resuming normal operations
    [Mon Jan 24 12:54:41.118315 2022] [core:notice] [pid 1190] AH00094: Command line: '/usr/sbin/httpd -D FOREGROUND'

<br>

## Config

`/etc/httpd/conf/httpd.conf`

- Every server has a configuration file. 
  - where you configure the different parameters that govern 
    - what port it listens on
    - where the static content is stored 
    - any SSL or HTTPS configurations,
    - where logs are stored

<br>

`ServerRoot "/etc/httpd"`

<br>


`port`: the Apache web server listens on all IPs of the hosts at port 80.

    #
    # Listen: Allows you to bind Apache to specific IP addresses and/or
    # ports, instead of the default. See also the <VirtualHost>
    # directive.
    #
    # Change this to Listen on specific IP addresses as shown below to 
    # prevent Apache from glomming onto all bound IP addresses.
    #
    #Listen 12.34.56.78:80
    Listen 80

<br>

`static`: `/var/www/html`

    #
    # DocumentRoot: The directory out of which you will serve your
    # documents. By default, all requests are taken from this directory, but
    # symbolic links and aliases may be used to point to other locations.
    #
    DocumentRoot "/var/www/html"

<br>
<br>

### Serving multiple applications

One Apache server can host multiple applications.
- It is a single server running a single web service, listening on the same IP address and same port which happens to be 80 in this case. This is to say, regardless of the DNS you are requesting on the web browser, the request is hitting the same server provided that it is configured on the Apache server. Apache will then redirect the request based on the DNS name used in the request and point to the correct path in the host server to return the right content. 

<br>

    /etc/httpd/conf/foodies.conf

    <VirtualHost *:80>
        ServerName www.foodies.com
        DocumentRoot /var/www/foodies
    </VirtualHost>

<br>

    /etc/httpd/conf/fruitties.conf

    <VirtualHost *:80>
        ServerName www.fruitties.com
        DocumentRoot /var/www/fruitties
    </VirtualHost>

<br>
<br>

Include all your applications in the `etc/httpd/conf/httpd.conf` file.

    #
    # Dynamic Shared Object (DSO) Support
    #
    # To be able to use the functionality of a module which was built as a DSO you
    # have to place corresponding `LoadModule' lines at this location so the
    # directives contained in it are actually available _before_ they are used.
    # Statically compiled modules (those listed by `httpd -l') do not need
    # to be loaded here.
    #
    # Example:
    # LoadModule foo_module modules/mod_foo.so
    #
    Include conf.modules.d/*.conf
    Include conf/foodies.conf
    Include conf/fruitties.conf

<br>
<br>

## Exercise

- `sudo mv /path/* /dest_path` : Moving multiple files at once

      sudo mv /opt/food/* /var/www/html/