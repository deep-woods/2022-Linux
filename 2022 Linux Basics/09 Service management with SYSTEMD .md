# Service management with SYSTEMD 

**Real-world problems**

1. Change request in production - patch +100 servers.
automation tool called ANSIBLE

Patching is a process to repair a vulnerability or a flaw that is identified after the release of an application or a software. Newly released patches can fix a bug or a security flaw, can help to enhance applications with new features, fix security vulnerability.

2. Make sure Django application works properly before switching over to the Dev Servers.

But the Django application will stop running as soon as you exit the terminal or repoot the system. How can we make it run in the background and ensure that it is up and running when the system boots?

**Solution**: create a SYSTEMD service for the application.

<br>
<br>

## Service management with `Sytemd`

**Example service****

|  Workflow  |  Systemd service action  |
| -- | -- |
| Program - `/user/bin/project-forest.sh`   |  • Create a service.<br>`[Service]`<br>`ExecStart= /bin/bash /usr/bin/project-forest.sh`<br>  >> `project-forest.service` created.<br><br> • Start service<br>`sudo systemctl start project-forest.service`<br>• Check status with `systemctl status project-forest.service`  |
| Start Python app after Postgres DB   |  • Edit `.service` file.<br>`[Unit]`<br>`Description=Python Django for Project Forest`<br>`Documentation=https://wiki/codingforestfoundation.org/forest` <br>`After=postgresql.service`  |
| Use `service account` project-forest   |  • Edit `.service` file.<br>`[Service]`<br>`User=project-forest`  |
| Auto restart on fail   | • Edit `.service` file.<br>`Restart=on-failure`   |
| Set restart interval to 10 seconds   |  • Edit `.service` file.<br>`RestartSec=10` |
| Log service events   | (automatically done without configuration)   |
| Load when booting into graphical mode   |  • Edit `.service` file.<br>`[Install]`<br>`WantedBy graphical.target`    |

<br>

`/etc/systemd/system/project-forest.service`

    [Unit]
    Description=Python Django for Project Forest
    Documentation=https://wiki/codingforestfoundation.org/forest
    After=postgresql.service

    [Service]
    ExecStart= /bin/bash /usr/bin/project-forest.sh
    User=project-forest
    Restart=on-failure
    RestartSec=10

    [Install]
    WantedBy graphical.target


<br>
 
- `systemctl daemon-reload`: make `systemd` aware of the changes in `.service` file without interrupting the running service.
  - A running service whose unit file has been updated on disk can only be restarted after running the systemctl daemon-reload command.
- `systemctl start project-forest.service`
- `systemctl edit`: make changes to the newly created unit file, (`.service`).
  - `--full`: opens the file in text editor. 
  - Units changed using `systemctl edit` command are applied immediately without having to run the daemon-reload.

        systemctl edit project-forest.service --full

- `systemctl get-default`: check the current `runlevel`
- `systemctl set-default`: change `runlevel` to a different target.

        systemctl set-default multi-user.target

- `systemctl list-units --all`


<br>
<br>

## `SYSTEMD` Tools

<br>

### **Systemctl**

|  `systemctl`  |  Command(s)  |
| -- | -- |
|  Manage system state  |    |
|  Start<br>Stop<br>Restart<br>Reload (without interruption,<br>service keeps running) | `systemctl start docker`<br>`systemctl stop docker`<br>`systemctl restart docker`<br>`systemctl reload dockere`   |
|  Enable<br>Disable  |  `systemctl enable docker`<br>`systemctl disable docker`   |
|  List and manage units  |    |
|  List and update targets  |    |

<br>

- `enable`: enable a service and make it persistent across reboot,
- `disable`: disable the service at boot

Some more states

- active: service running
- inactive: service stopped
- failed: crashed/error/timeout

<br>

### **Journalctl**

<br>

- a convenient troubleshooting tool to figure out issues such as service failures.
- used to query systemd journal 

<br>

|   Command(s) |  Description |
| -- | -- |
|  `journalctl`  | prints all the log entries from the oldest entry to the newest  |
| `journalctl -b` | see the logs from the current boot |
| `journalctl -u` |  see the log entries for a specific unit | 

<br>
<br>

### **Exercise**

1. Check the status of your service. 

       systemctl status forest.service

        ● forest.service - Modular project for forest protection. Watches over individual tree health.  
            Loaded: error (Reason: Invalid argument)
            Active: inactive (dead)

        Jan 23 08:02:25 caleston-lp10 systemd[1]: forest.service: Service lacks both ExecStart= and
        Jan 23 08:04:24 caleston-lp10 systemd[1]: forest.service: Service lacks both ExecStart= and
        Jan 23 08:05:28 caleston-lp10 systemd[1]: forest.service: Service lacks both ExecStart= and

<br>

2. Try running the service. 

        sudo systemctl start forest.service

        Failed to start forest.service: Unit forest.service is not loaded properly: Invalid argument.
        See system logs and 'systemctl status forest.service' for details.

Check the system log above in 1. 
Now, edit the `.service` file so that the service can run smoothly. 


        sudo vi forest.service

        forest@forest-repo-10:/etc/systemd/system$ cat forest.service

        [Unit]
        Description=A template service unit file. Use this to create a service

        [Service]
        ExecStart= /bin/bash /root/forest_script.sh

        [Install]
        WantedBy=multi-user.target

Check the status again to make sure the service is not up an running. 

        systemctl status forest.service
        ● forest.service - A template service unit file. Use this to create a service
            Loaded: loaded (/etc/systemd/system/forest.service; disabled; vendor preset: enabled)
            Active: active (running) since Sun 2022-01-23 08:08:27 UTC; 9s ago
        Main PID: 1081 (bash)
            Tasks: 2 (limit: 5529)
        CGroup: /docker/65sdfsd...9da217c5219e87f/system.
                ├─1081 /bin/bash /root/forest_script.sh
                └─1083 sleep 100

<br>

3. Enable the service for auto-start on reboot.

        sudo systemctl enable forest.service

        Created symlink /etc/systemd/system/multi-user.target.wants/forest.service → /etc/systemd/system/forest.service.

<br>

4. Check the log to inspect errors and warnings. 

        sudo journalctl -u forest.service

        -- Logs begin at Sun 2022-01-23 08:01:46 UTC, end at Sun 2022-01-23 08:15:17 UTC. --
        Jan 23 08:02:25 caleston-lp10 systemd[1]: forest.service: Service lacks both ExecStart= and
        Jan 23 08:04:24 caleston-lp10 systemd[1]: forest.service: Service lacks both ExecStart= and
        Jan 23 08:05:28 caleston-lp10 systemd[1]: forest.service: Service lacks both ExecStart= and
        Jan 23 08:08:27 caleston-lp10 bash[1081]: Sun Jan 23 08:08:27 UTC 2022
        Jan 23 08:10:07 caleston-lp10 bash[1081]: Sun Jan 23 08:10:07 UTC 2022
        Jan 23 08:11:47 caleston-lp10 bash[1081]: Sun Jan 23 08:11:47 UTC 2022
        Jan 23 08:13:27 caleston-lp10 bash[1081]: Sun Jan 23 08:13:27 UTC 2022
        Jan 23 08:15:07 caleston-lp10 bash[1081]: Sun Jan 23 08:15:07 UTC 2022
