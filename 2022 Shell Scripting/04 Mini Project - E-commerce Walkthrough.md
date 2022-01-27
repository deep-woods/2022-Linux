# <span id='top'>04 Mini Project - E-commerce Walkthrough</span>

<br>

[[Firewall]](#firewall)   
[[MariaDB]](#mariadb)   
[[php]](#php)   

<br>
<br>

## <span id='firewall'>`Firewall`</span>

[[:point_up: top]](#top)

<br>

`sudo yum install -y firewalld`

<br>

    Installed:
        firewalld.noarch 0:0.6.3-13.el7_9                                                                                    
    Dependency Installed:
        ebtables.x86_64 0:2.0.10-16.el7                         firewalld-filesystem.noarch 0:0.6.3-13.el7_9                ipset.x86_64 0:7.1-1.el7                                ipset-libs.x86_64 0:7.1-1.el7                               
        libselinux-python.x86_64 0:2.5-15.el7                   python-decorator.noarch 0:3.4.0-3.el7                       
        python-firewall.noarch 0:0.6.3-13.el7_9                 python-slip.noarch 0:0.4.0-4.el7                            
        python-slip-dbus.noarch 0:0.4.0-4.el7                  

    Dependency Updated:
        libselinux.x86_64 0:2.5-15.el7        

`sudo service firewalld start`

    Redirecting to /bin/systemctl start firewalld.service

`sudo systemctl enable firewalld`

<br>

## <span id='mariadb'>MariaDB</span>

[[:point_up: top]](#top)

Setup MariaDB

`sudo yum install -y mariadb-server`

    Installed:
        mariadb-server.x86_64 1:5.5.68-1.el7                                                                                 

    Dependency Installed:
        groff-base.x86_64 0:1.22.2-8.el7                           libaio.x86_64 0:0.3.109-13.el7                           
        mariadb.x86_64 1:5.5.68-1.el7                              mariadb-libs.x86_64 1:5.5.68-1.el7                       
        perl.x86_64 4:5.16.3-299.el7_9                             perl-Carp.noarch 0:1.26-244.el7                          
        perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7               perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7              
        perl-DBD-MySQL.x86_64 0:4.023-6.el7                        perl-DBI.x86_64 0:1.627-4.el7                            
        perl-Data-Dumper.x86_64 0:2.145-3.el7                      perl-Encode.x86_64 0:2.51-7.el7                          
        perl-Exporter.noarch 0:5.68-3.el7                          perl-File-Path.noarch 0:2.09-2.el7                       
        perl-File-Temp.noarch 0:0.23.01-3.el7                      perl-Filter.x86_64 0:1.49-3.el7                          
        perl-Getopt-Long.noarch 0:2.40-3.el7                       perl-HTTP-Tiny.noarch 0:0.033-3.el7                      
        perl-IO-Compress.noarch 0:2.061-2.el7                      perl-Net-Daemon.noarch 0:0.48-5.el7                      
        perl-PathTools.x86_64 0:3.40-5.el7                         perl-PlRPC.noarch 0:0.2020-14.el7                        
        perl-Pod-Escapes.noarch 1:1.04-299.el7_9                   perl-Pod-Perldoc.noarch 0:3.20-4.el7                     
        perl-Pod-Simple.noarch 1:3.28-4.el7                        perl-Pod-Usage.noarch 0:1.63-3.el7                       
        perl-Scalar-List-Utils.x86_64 0:1.27-248.el7               perl-Socket.x86_64 0:2.010-5.el7                         
        perl-Storable.x86_64 0:2.45-3.el7                          perl-Text-ParseWords.noarch 0:3.29-4.el7                 
        perl-Time-HiRes.x86_64 4:1.9725-3.el7                      perl-Time-Local.noarch 0:1.2300-2.el7                    
        perl-constant.noarch 0:1.27-2.el7                          perl-libs.x86_64 4:5.16.3-299.el7_9                      
        perl-macros.x86_64 4:5.16.3-299.el7_9                      perl-parent.noarch 1:0.225-244.el7                       
        perl-podlators.noarch 0:2.5.1-3.el7                        perl-threads.x86_64 0:1.87-4.el7                         
        perl-threads-shared.x86_64 0:1.43-6.el7                   

    Complete!

`service firewalld status`

    Redirecting to /bin/systemctl status firewalld.service
    ● firewalld.service - firewalld - dynamic firewall daemon
        Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2022-01-27 13:51:12 UTC; 6min ago
            Docs: man:firewalld(1)
    Main PID: 1040 (firewalld)
        CGroup: /docker/d11887dbbee9b89b3121dffb53a2fb8208eaaa00127e81c90a0033fdf4830d90/system.slice/firewalld.service
            └─1040 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid


`sudo vi /etc/my.cnf`
: Make no changes for now.


    [mysqld]
    datadir=/var/lib/mysql
    socket=/var/lib/mysql/mysql.sock
    # Disabling symbolic-links is recommended to prevent assorted security risks
    symbolic-links=0
    # Settings user and group are ignored when systemd is used.
    # If you need to run mysqld under a different user or group, 
    # customise your systemd unit file for mariadb according to the 
    # instructions in http://fedoraproject.org/wiki/Systemd

    [mysqld_safe]
    log-error=/var/log/mariadb/mariadb.log
    pid-file=/var/run/mariadb/mariadb.pid

`sudo service mariadb start`

    Redirecting to /bin/systemctl start mariadb.service

`sudo systemctl enable mariadb`

    Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.

`sudo service mariadb status`

    edirecting to /bin/systemctl status mariadb.service
    ● mariadb.service - MariaDB database server
        Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
        Active: active (running) since Thu 2022-01-27 14:09:14 UTC; 53s ago
    Main PID: 1636 (mysqld_safe)
        CGroup: /docker/95a8ce20c618d03032530cda7efac13f15e1f17e5c7f9270b6932a56fc41a0ff/system.slice/mariadb.service
            ├─1636 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
            └─1800 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64...

    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: PLEASE REMEMBER TO SET A PASSWORD FOR T... !
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: To do so, start the server, then issue ...s:
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: '/usr/bin/mysqladmin' -u root password ...d'
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: '/usr/bin/mysqladmin' -u root -h calest...d' 
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: Alternatively you can run:
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: '/usr/bin/mysql_secure_installation'
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: which will also give you the option of ...st
    Jan 27 14:09:12 caleston-lp10 mariadb-prepare-db-dir[1555]: databases and anonymous user created by...is
    Jan 27 14:09:12 caleston-lp10 mysqld_safe[1636]: 220127 14:09:12 mysqld_safe Logging to '/var/log/...g'.
    Jan 27 14:09:12 caleston-lp10 mysqld_safe[1636]: 220127 14:09:12 mysqld_safe Starting mysqld daemo...sql
    Hint: Some lines were ellipsized, use -l to show in full.

<br>

## Configure firewall for Database.

`sudo firewall-cmd --list-all`

    public
        target: default
        icmp-block-inversion: no
        interfaces: 
        sources: 
        services: dhcpv6-client ssh
        ports:      <<---- (we are configuring this.)
        protocols: 
        masquerade: no
        forward-ports: 
        source-ports: 
        icmp-blocks: 
        rich rules: 

`sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp`

    success

`sudo firewall-cmd --reload`

    success

`sudo firewall-cmd --list-all`

    public
        target: default
        icmp-block-inversion: no
        interfaces: 
        sources: 
        services: dhcpv6-client ssh
        ports: 3306/tcp         <<---- (now it's been successfully configured.)
        protocols: 
        masquerade: no
        forward-ports: 
        source-ports: 
        icmp-blocks: 
        rich rules: 

<br>

`sudo mysql`

        MariaDB [(none)]> CREATE DATABASE ecomdb;
        Query OK, 1 row affected (0.00 sec)


        MariaDB [(none)]> CREATE DATABASE ecomdb;
        Query OK, 1 row affected (0.00 sec)


        MariaDB [(none)]> SHOW DATABASES;
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | ecomdb             |
        | mysql              |
        | performance_schema |
        | test               |
        +--------------------+
        5 rows in set (0.00 sec)


        MariaDB [(none)]> CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
        Query OK, 0 rows affected (0.00 sec)


        MariaDB > GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
        Query OK, 0 rows affected (0.00 sec)

        
        MariaDB > FLUSH P
        Query OK, 0 rows affected (0.00 sec)

<br>

- load inventory data
: this will appear in the application once it's all up and running.

        cat > db-load-script.sql

        USE ecomdb;                                                                                 
        CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;

        INSERT INTO products (Name,Price,ImageUrl) VALUES ("Laptop","100","c-1.png"),("Drone","200","c-2.png"),("VR","300","c-3.png"),("Tablet","50","c-5.png"),("Watch","90","c-6.png"),("Phone Covers","20","c-7.png"),("Phone","80","c-8.png"),("Laptop","150","c-4.png");

        ^C

`sudo mysql`

        SHOW DATABASES;
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | ecomdb             |   << We want this one.
        | mysql              |
        | performance_schema |
        | test               |
        +--------------------+
        5 rows in set (0.00 sec)


        MariaDB [(none)]> use ecomdb
        Reading table information for completion of table and column names
        You can turn off this feature to get a quicker startup with -A
        Database changed


        MariaDB [ecomdb]> SELECT * FROM products;
        +----+--------------+-------+----------+
        | id | Name         | Price | ImageUrl |
        +----+--------------+-------+----------+
        |  1 | Laptop       | 100   | c-1.png  |
        |  2 | Drone        | 200   | c-2.png  |
        |  3 | VR           | 300   | c-3.png  |
        |  4 | Tablet       | 50    | c-5.png  |
        |  5 | Watch        | 90    | c-6.png  |
        |  6 | Phone Covers | 20    | c-7.png  |
        |  7 | Phone        | 80    | c-8.png  |
        |  8 | Laptop       | 150   | c-4.png  |
        +----+--------------+-------+----------+
        8 rows in set (0.00 sec)


<br>

## <span id='php'>`php`</span>

[[:point_up: top]](#top)

`sudo yum install -y httpd php php-mysql`

    Installed:
        httpd.x86_64 0:2.4.6-97.el7.centos.4   php.x86_64 0:5.4.16-48.el7   php-mysql.x86_64 0:5.4.16-48.el7  

    Dependency Installed:
        apr.x86_64 0:1.4.8-7.el7                           apr-util.x86_64 0:1.5.2-6.el7                      
        centos-logos.noarch 0:70.0.6-3.el7.centos          httpd-tools.x86_64 0:2.4.6-97.el7.centos.4         
        libedit.x86_64 0:3.0-12.20121213cvs.el7            libzip.x86_64 0:0.10.1-8.el7                       
        mailcap.noarch 0:2.1.41-2.el7                      php-cli.x86_64 0:5.4.16-48.el7                     
        php-common.x86_64 0:5.4.16-48.el7                  php-pdo.x86_64 0:5.4.16-48.el7         

<br>

`sudo firewall-cmd --permanent --zone=public --add-port=80/tcp`
    
    success

`sudo firewall-cmd --reload`

    success

`sudo firewall-cmd --list-all`

    public
        target: default
        icmp-block-inversion: no
        interfaces: 
        sources: 
        services: dhcpv6-client ssh
        ports: 3306/tcp 80/tcp         <<------ (port 80/tcp has been successfully added.)
        protocols: 
        masquerade: no
        forward-ports: 
        source-ports: 
        icmp-blocks: 
        rich rules: 


## Configure `httpd`

1) You can either: 

`sudo vi /etc/httpd/conf/httpd.conf`

        /DirectoryIndex        <<----------------- (Search for this attribute)

        #
        # DirectoryIndex: sets the file that Apache will serve if a directory
        # is requested.
        #
        <IfModule dir_module>
            DirectoryIndex index.html       <<------------- Modify this.
        </IfModule>


2) 

        sudo vi /etc/httpd/conf/httpd.conf

<br>
<br>


## Start `httpd` (`apache` server)

`sudo service httpd start`

        Redirecting to /bin/systemctl start httpd.service

`sudo systemctl enable httpd`

        Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.

`sudo service httpd status`

        Redirecting to /bin/systemctl status httpd.service
        ● httpd.service - The Apache HTTP Server
            Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
            Active: active (running) since Thu 2022-01-27 14:41:06 UTC; 29s ago
                Docs: man:httpd(8)
                    man:apachectl(8)
        Main PID: 2443 (httpd)
            Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
            CGroup: /docker/95a8ce20c618d03032530cda7efac13f15e1f17e5c7f9270b6932a56fc41a0ff/system.slice/httpd.service
                ├─2443 /usr/sbin/httpd -DFOREGROUND
                ├─2445 /usr/sbin/httpd -DFOREGROUND
                ├─2446 /usr/sbin/httpd -DFOREGROUND
                ├─2447 /usr/sbin/httpd -DFOREGROUND
                ├─2448 /usr/sbin/httpd -DFOREGROUND
                └─2449 /usr/sbin/httpd -DFOREGROUND

## Test your server

1) `Apache` server on default template is running. But not quite our app.

`curl http://localhost`

        <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd"><html><head>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
                        <title>Apache HTTP Server Test Page powered by CentOS</title>
                        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

            <!-- Bootstrap -->
            <link href="/noindex/css/bootstrap.min.css" rel="stylesheet">
            <link rel="stylesheet" href="noindex/css/open-sans.css" type="text/css" />

        <style type="text/css"><!--              


<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Server_apache_default.png" width=600>

<br>
<br>

2) Download the application and configure the `database server IP` & `port`. 

    sudo yum install -y git
    git clone https://github.com/kodekloudhub/learning-app-ecommerce.git /var/www/html/

    cd /var/www/html ; ls
    assets  css  fonts  img  index.php  js  README.md  scss  vendors

    (We want this file up there ⬆)

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/images/Server_apache_ecommerce.png" width=600>