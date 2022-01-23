# Troubleshooting in Development

**Real world problems**

The application I developed is working on my laptop. But it breaks when run on company server. Why...?

- Network?
- Permission issue? 

<br>

Possible reasons

- `Code`: it's working on my laptop. So rule it out.
- `development environment`: break down to working vs breaking environments.
  - `network`
    - check the connectivity between the app and the DB server.
    - are the ports open?
    - run a telnet check.
  - `access permission`
    - check which user and user permissions.
  - `environmental dependencies`:
  - `configuration issues`:


<br>
<br>

## Exercise

1. Move your code from your laptop over to your company's server. 

On your laptop: 

      forest@forest-walker01: ~$ scp forest-code.tar.gz forest-dev01:/home/forest
      
      forest-code.tar.gz                    100% 1439KB 132.9MB/s   00:00   


<br>

2. Unzip your code and extract it over to `/prod/` on your company host. 

        forest@forest-walker01: ~$ ssh forest-dev01

        forest@forest-dev01: ~$ tar -xvf forest-code.tar.gz
        forest-code

        sudo mv forest-code /opt 

<br>

3. Remove the `tar` file.

        sudo rm forest-code.tar.gz

<br>

4. Connect to `forest-db-01 server` to check the status of the `pstgresql.service` for the `postgres db`.

        ssh forest-db-01

        systemctl status postgresql.service
        ● postgresql.service - PostgreSQL RDBMS
          Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
          Active: inactive (dead) since Sun 2022-01-23 15:33:51 UTC; 23min ago
          Process: 5702 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
        Main PID: 5702 (code=exited, status=0/SUCCESS)

        Warning: Journal has been rotated since unit was started. Log output is incomplete or unavailable.

<br>

5. Modify the replication privilege for the `postgres db`. 

`pg_hba`: **PostGres Host-Based Authentication**. Client authentication is controlled by a configuration file, which traditionally is named pg_hba.conf and is stored in the database cluster's data directory. (HBA stands for host-based authentication.) A default pg_hba.conf file is installed when the data directory is initialized by initdb. It is possible to place the authentication configuration file elsewhere, however; see the hba_file configuration parameter.

      sudo vi /etc/postgresql/10/main/pg_hba.conf
      --INSERT-- host    all             all             0.0.0.0/0 md5

      # DO NOT DISABLE!
      # If you change this first entry you will need to make sure that the
      # database superuser can access the database using some other method.
      # Noninteractive access to all databases is required during automatic
      # maintenance (custom daily cronjobs, replication, and similar tasks).
      #
      # Database administrative login by Unix domain socket
      local   all             postgres                                peer

      # TYPE  DATABASE        USER            ADDRESS                 METHOD

      # "local" is for Unix domain socket connections only
      local   all             all                                     peer
      # IPv4 local connections:
      host    all             all             127.0.0.1/32            md5
      # IPv6 local connections:
      host    all             all             ::1/128                 md5
      # Allow replication connections from localhost, by a user with the
      # replication privilege.
      local   replication     all                                     peer
      host    replication     all             127.0.0.1/32            md5
      host    replication     all             ::1/128                 md5
      host    all             all             0.0.0.0/0 md5


<br>

6. Start the `postgres` db on the dev server. 

        systemctl start postgresql
        Failed to start postgresql.service: The name org.freedesktop.PolicyKit1 was not provided by any .service files
        See system logs and 'systemctl status postgresql.service' for details.

        sudo systemctl start postgresql

<br>

7. Check the port the `postgresql` db is running on.

        netstat -natulp | grep postgres | gre LISTEN

        0 0.0.0.0:5432            0.0.0.0:*               LISTEN      6010/postgres       
        tcp6       0      0 :::5432                 :::*                    LISTEN      6010/postgres       
        forest@devdb01:~$ 

8. Run the `Django` web application..

        cd /opt/forest-code/ProjectForest

        /opt/forest-code/ProjectForest$ python3 manage.py runserver 0.0.0.0:8000
        Watching for file changes with StatReloader
        Performing system checks...

        System check identified no issues (0 silenced).

        You have 19 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, blog, contenttypes, portfolios, sessions.
        Run 'python manage.py migrate' to apply them.

        January 23, 2022 - 16:14:21
        Django version 2.2.6, using settings 'forest.settings'
        Starting development server at http://0.0.0.0:8000/
        Quit the server with CONTROL-C.

<br>

9. Why did the app fail to run? What was the problem? 

<br>

10. 

I forgot to configure my app to connect to `postgres` db running on different server. 
While the `postgres` db in my laptop resided in the same system as the `app`, the dev server only hosts the `app`, and the `postgres` db is living elsewhere (on `forest-db-01` server). I forgot to address this connection config before starting the app on `dev server` :P ...

<br>

|  My laptop  | Dev server  | DB server |
| -- | -- | -- |
| • `app`<br>• `db`   | • `app`   | |
|    |    | • `db`  |

Q. In `/opt/forest-code` directory, find the `DATABASES` configuration data and change the host information to `forest-db-01`. 


<br>

****


- ``:
- ``:
- ``:

<br>

### References

  - Kodekloud.com
  - Postgres (n.d.) 19.1. The pg_hba.conf File https://www.postgresql.org/docs/9.1/auth-pg-hba-conf.html