## Issue - Permission denied

I try to run docker and it wouldn't run. Permission denied.

        $ docker run docker/whalesay cowsay "let's do this!"

            docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
            See 'docker run --help'.

<br>

## Reason

This is becuase the current user doesn't have the permisson to access the directory `/var/run/docker.sock`. The user should be granted the `root:docker` permission.

<br>

## Solution

        $ id
        uid=1000(forest) gid=1000(forest) groups=1000(forest),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),132(lxd),133(sambashare)

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
                    ├─ 31785 dbus-launch --autolaunch=edb660515b0b4778aebf6ec05b732d40 --binary-syntax --close-stderr
                    ├─ 31786 /usr/bin/dbus-daemon --syslog-only --fork --print-pid 5 --print-address 7 --session
                    ├─ 32434 dbus-launch --autolaunch=edb660515b0b4778aebf6ec05b732d40 --binary-syntax --close-stderr
                    ├─ 32435 /usr/bin/dbus-daemon --syslog-only --fork --print-pid 5 --print-address 7 --session
                    └─104687 /usr/bin/anydesk --backend

<br>

### References

- https://technote.kr/369
