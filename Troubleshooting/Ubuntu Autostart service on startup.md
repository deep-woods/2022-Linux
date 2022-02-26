## Issue

Need to reboot a remotely controlled computer for permission update. Need to guarantee that I can control the machine remotely after reboot.

<br>

## Solution

Enable the `anydesk.service` which will auto start the service on startup.

        $ systemctl enable anydesk.service
        $ systemctl status anydesk

        ● anydesk.service - AnyDesk
            Loaded: loaded (/etc/systemd/system/anydesk.service; enabled; vendor preset: enabled)
            Active: active (running) since Thu 2022-02-24 17:28:23 KST; 2 days ago
        Main PID: 31456 (anydesk)
            Tasks: 45 (limit: 38374)
            Memory: 69.2M
            CGroup: /system.slice/anydesk.service
                    ├─ 31456 /usr/bin/anydesk --service
                    ├─ 31470 /usr/bin/anydesk --tray
                    ├─ 31785 dbus-launch --autolaunch=edb660 --binary-syntax --close-stderr
                    ├─ 31786 /usr/bin/dbus-daemon --syslog-only --fork --print-pid 5 --print-address 7 --session
                    ├─ 32434 dbus-launch --autolaunch=edb660 --binary-syntax --close-stderr
                    ├─ 32435 /usr/bin/dbus-daemon --syslog-only --fork --print-pid 5 --print-address 7 --session
                    └─104687 /usr/bin/anydesk --backend

<br>

### References

- https://askubuntu.com/questions/1305405/how-to-disable-anydesk-autostart-in-ubuntu
