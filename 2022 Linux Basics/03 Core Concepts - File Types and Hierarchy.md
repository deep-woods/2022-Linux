# File Types and Hierarchy

<br>

**File types**

- Regular file: images, scripts, configuration/data files
- Directory: /home/Forest 
- Special files: 

  - `/dev/`: `character files`, `block files` (hard disks, RAM)
  - `links`, `socket files`, `named files`

- `sudo file /path/file.name`: check file type

      file ~/.City.txt
      America/Peru/Lima/Miraflores/City.txt: ASCII text
      
      sudo file /root/firefox.deb
      /root/firefox.deb: Debian binary package (format 2.0
      
      sudo file /root/sample_script.sh
      /root/sample_script.sh: Bourne-Again shell script, ASCII text executable

<br>

**Filesystem Hierarchy**

- `df -hP`: report file system disk space usage

        Filesystem      Size  Used Avail Use% Mounted on
        udev            1.9G     0  1.9G   0% /dev
        tmpfs           390M  1.9M  388M   1% /run
        /dev/sda5        20G   13G  5.5G  70% /
        tmpfs           2.0G  133M  1.8G   7% /dev/shm
        tmpfs           5.0M  4.0K  5.0M   1% /run/lock
        tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
        /dev/loop0       56M   56M     0 100% /snap/core18/2284
        /dev/loop1      248M  248M     0 100% /snap/gnome-3-38-2004/87
        /dev/loop2       66M   66M     0 100% /snap/gtk-common-themes/1519
        /dev/loop3       66M   66M     0 100% /snap/gtk-common-themes/1515
        /dev/loop4      219M  219M     0 100% /snap/gnome-3-34-1804/72
        /dev/loop5       56M   56M     0 100% /snap/core18/2128
        /dev/loop6       51M   51M     0 100% /snap/snap-store/547
        /dev/loop7       55M   55M     0 100% /snap/snap-store/558
        /dev/loop8       36M   36M     0 100% /snap/pinta/18
        /dev/loop9       33M   33M     0 100% /snap/snapd/12704
        /dev/loop10     219M  219M     0 100% /snap/gnome-3-34-1804/77
        /dev/loop11      44M   44M     0 100% /snap/snapd/14295
        /dev/loop12     128K  128K     0 100% /snap/bare/5
        /dev/loop13      62M   62M     0 100% /snap/core20/1270
        /dev/sda1       511M  4.0K  511M   1% /boot/efi
        tmpfs           390M   36K  390M   1% /run/user/1000

**`init` system**

- `ls -l /sbin/init`

      lrwxrwxrwx 1 root root 20 Jan  9 20:56 /sbin/init -> /lib/systemd/systemd
