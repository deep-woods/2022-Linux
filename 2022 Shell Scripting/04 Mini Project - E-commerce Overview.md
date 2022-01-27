# 04 Mini Project - E-commerce Overview

<br>

## Project e-commerce 

- The whole code and app architecture from [Kodekloud](https://github.com/kodekloudhub/shell-scripting-for-beginners-course/blob/master/docs/05-Project-E-Commerce-Application/01-E-Commerce-Application-Introduction.md)
- Deployment model: single node
- Multi-node setup: 
  - `172.20.1.101` - `MariaDB`
  - `172.20.1.102` - `apache`, `php`
- Setup differences according to the node types: configuration and connectivity

| DB server<br>`172.20.1.101`    |  App server<br>`172.20.1.102`  |
| -- | -- |
|  `MariaDB`  |  `apache`, `php`  |
| mysql<br>MariaDB > CREATE DATABASE ecomdb;<br>MariaDB > CREATE USER 'ecomuser'@'`172.20.1.102`' IDENTIFIED BY 'ecompassword';<br>MariaDB > GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'`172.20.1.102`;<br>MariaDB > FLUSH PRIVILEGES; | <? php<br>$link = mysqli_connect('`172.20.1.101`', 'ecomuser', 'ecompasword'...) | 


<br>

## Setup

<br>

### Firewall

<br>

Firewall configuration

    sudo yum install -y firewalld
    sudo service firewalld start
    sudo systemctl enable firewalld

<br>

### MariaDB

Setup MariaDB

- install

        sudo yum install -y mariadb-server

- prot configuration

        sudo vi /etc/my.cnf  

- start db 

        sudo service mariadb start

- enable db on reboot

        sudo systemctl enable mariadb

- configure DB firewall

        sudo firewall-cmd --permanent --zone=public --add-port=3306.tcp
        sudo firewall-cmd --reload

- coinfugre DB

        mysql
        MariaDB > CREATE DATABASE ecomdb;
        MariaDB > CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
        MariaDB > GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
        MariaDB > FLUSH P

- load data

        mysql < db-load-script.sql

<br>

### php

Install `apache`, `php`

        sudo yum install -y httpd php php-mysql

Configure `firewall`

        sudo firewall-cmd --permanent --zone=public --add-port=80/tcp
        sudo firewall-cmd --reload

Configure `httpd`
: configure DirectoryIndex to use`index.php` instaed of `index.html`

        sudo vi /etc/httpd/conf/httpd.conf

Start `httpd`

        sudo service httpd start
        sudo systemctl enable httpd
    
Download code

        sudo yum install -y git
        git clone https://github.com/<application>.git /var/www/html

Test

        curl http://localhost