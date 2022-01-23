# Storage

<br>

### **Real-world problems**

You ahve to migrate the Django project from your laptop to dev server.

<br>

**Challenges**

- home directory is 95% full
- all the mounts in the DF command added up to about 50GB in total.


<br>
<br>

## Storage basics

- Disk partitions
- Linux filesystems (EXT2-EXT4)
- NFS
- External Storage Devices (DAS, NAS, SAN)
- LVM (Logical Volume Manager)

<br>
<br>

- Block device: a type of file found under the /dev/ directory. 
  - A piece of hardware that can store data. Data is read or written to it in blocks or chunks of space.
    - SSD (solid-state disk)
    - Traditional spinning hard disk
  - `sda`: represents the entire disk. 

<br>

**Checking block storage**

    lsblk

    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    loop0    7:0    0  55.4M  1 loop /snap/core18/2128
    loop1    7:1    0  54.2M  1 loop /snap/snap-store/558
    loop2    7:2    0   219M  1 loop /snap/gnome-3-34-1804/77
    loop3    7:3    0  35.7M  1 loop /snap/pinta/18
    loop4    7:4    0   219M  1 loop /snap/gnome-3-34-1804/72

    sda      8:0    0    20G  0 disk 
    ├─sda1   8:1    0   1024M  0 part /boot/efi
    ├─sda2   8:2    0     1K  0 part 
    └─sda5   8:5    0  19.5G  0 part /


<br>

    ls -l /dev/ | grep "^b"

    brw-rw----  1 root      disk      7,   0 Jan 23 09:27 loop0
    brw-rw----  1 root      disk      7,   1 Jan 23 09:27 loop1
    brw-rw----  1 root      disk      7,  10 Jan 23 09:27 loop10
    brw-rw----  1 root      disk      7,  11 Jan 23 09:27 loop11
    brw-rw----  1 root      disk      7,  12 Jan 23 09:27 loop12
    brw-rw----  1 root      disk      7,  13 Jan 23 09:27 loop13
    brw-rw----  1 root      disk      7,  14 Jan 23 09:33 loop14
    brw-rw----  1 root      disk      7,   2 Jan 23 09:27 loop2

<br>
<br>


## Storage partition

Partitioning allows user to segment space and use each partition for a specific purpose.

**Partition table**

    lsblk

    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    ...
    sda      8:0    0    20G  0 disk 
    ├─sda1   8:1    0   1024M  0 part /boot/efi
    ├─sda2   8:2    0     1K  0 part 
    └─sda5   8:5    0  19.5G  0 part /

<br>

- `Major`: type of block device.
- `Minor`: used to distinguish individual, physical or logical devices

<br>

- `Mountpoint`: indicates where the storage is mounted to. In the example above, `sda5` is dedicated to `/root`. 
- `Mountpoint - /boot/efi`:  consumed as a system partition mounted at /boot/EFI; used during the system boot process and contains the boot loaders
for the installed OS.

<br>

**`fdisk`**

- list the partition table information; used to create and delete partitions.
- `fdisk -l`: list the partition table

        sudo fdisk -l /dev/sda

        Disk /dev/sda: 20 GiB, 21474836480 bytes, 41943040 sectors
        Disk model: VMware Virtual S
        Units: sectors of 1 * 1024 = 1024 bytes
        Sector size (logical/physical): 1024 bytes / 1024 bytes
        I/O size (minimum/optimal): 1024 bytes / 1024 bytes
        Disklabel type: dos
        Disk identifier: 0x1a89c3aa

        Device     Boot   Start      End  Sectors  Size Id Type
        /dev/sda1  *       2048  1050623  1048576  1024M  b W95 FAT32
        /dev/sda2       1052670 41940991 40888322 19.5G  5 Extended
        /dev/sda5       1052672 41940991 40888320 19.5G 83 Linux


<br>

### Partition types

- Primary partition: used to boot an operating system. Traditionally, disks were limited to not more than four
primary partitions per disk.
- Extended partition: cannot be used on its own but can host logical partitions; create extended partitions and carve out logical partitions inside it. an extended partition is like a disk drive in its own right. This has a partition table that points to one or more logical partitions.
- Logical partition: created within an extended partition.

<br>

### Partition table

- MBR partitioning scheme: Master Boot Record. 
  - there can only be four primary partitions in MBR.
  -  the maximum size per disc is two terabytes.
- GPT:  G-U-I-D partition table
  -  created to address the limitations in MBR.
  - an unlimited number of partitions per disc. Only the OS's restrictions are the limit. 
  - Unlike MBR (max 2 TB), GPT doesn't have any disk size limit.

<br>
<br>

## **Exercise - Create partitions**

1. Look into how Forest's disk space is organised. 

        lsblk

        NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
        vda                    252:0    0   10G  0 disk 
        └─vda1                 252:1    0   10G  0 part 
        ├─vagrant--vg-root   253:0    0    9G  0 lvm  /
        └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
        vdforest                    252:16   0    1G  0 disk 
        vdc                    252:32   0    1G  0 disk 


<br>

2. Create a GPT Ppartition named vdforest of size 10GB on disk `dev/vdf`.

       sudo apt install gdisk

- Start the partitioning process.
  - `gdisk <target path>`

        gdisk /dev/vdf

        GPT fdisk (gdisk) version 1.0.3

        Partition table scan:
        MBR: not present
        BSD: not present
        APM: not present
        GPT: not present

        Creating new GPT entries.

- `?` for help.

        Command (? for help): ?
        b       back up GPT data to a file
        c       change a partition's name
        d       delete a partition
        i       show detailed information on a partition
        l       list known partition types
        n       add a new partition
        o       create a new empty GUID partition table (GPT)
        p       print the partition table
        q       quit without saving changes
        r       recovery and transformation options (experts only)
        s       sort partitions
        t       change a partition's type code
        v       verify disk
        w       write table to disk and exit
        x       extra functionality (experts only)
        ?       print this menu

- `n` to add a new partition.

        Command (? for help): n

        Partition number (1-128, default 1): 1
        First sector (34-2097118, default = 2048) or {+-}size{KMGTP}: 2048
        Last sector (2048-2097118, default = 2097118) or {+-}size{KMGTP}: +500M
        Current type is 'Linux filesystem'
        Hex code or GUID (L to show codes, Enter = 8300): 8300
        Changed type of partition to 'Linux filesystem'


- `w` to write table to disk and exit:

        Command (? for help): w

        Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
        PARTITIONS!!

        Do you want to proceed? (Y/N): y
        OK; writing new GUID partition table (GPT) to /dev/vdforest.
        The operation has completed successfully.

<br>

- Final check to ensure the operation result. 

- `lsblk`

        NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
        vda                    252:0    0   10G  0 disk 
        └─vda1                 252:1    0   10G  0 part 
        ├─vagrant--vg-root   253:0    0    9G  0 lvm  /
        └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
        vdf                    252:16   0    1G  0 disk 
        └─vdforest             252:17   0    1G  0 part 
        vdc                    252:32   0    1G  0 disk

<br>

- `sudo fdisk -l /dev/vdforest`

        Disk /dev/vdforest: 1 GiB, 1073741824 bytes, 2097152 sectors
        Units: sectors of 1 * 1024 = 1024 bytes
        Sector size (logical/physical): 1024 bytes / 1024 bytes
        I/O size (minimum/optimal): 1024 bytes / 1024 bytes
        Disklabel type: gpt
        Disk identifier: BAB1586C-15BD-4790-8EFF-8116D4437CB8

        Device           Start     End Sectors  Size Type
        /dev/vdforest1   2048 1026047  1024000  1GB Linux filesystem


<br>
<br>

**Other**

- `Command (? for help): p`
  - Look at the table. There is no partition at the moment. 

        Disk /dev/vdforest: 2097152 sectors, 1024.0 MiB
        Sector size (logical/physical): 1024/1024 bytes
        Disk identifier (GUID): 8A813A99-0988-408B-BBC1-5E647FDC3DE3
        Partition table holds up to 128 entries
        Main partition table begins at sector 2 and ends at sector 33
        First usable sector is 34, last usable sector is 2097118
        Partitions will be aligned on 2048-sector boundaries
        Total free space is 2097085 sectors (1024.0 MiB)

        Number  Start (sector)    End (sector)  Size       Code  Name


- `l` to list known partition types:


        Command (? for help): l
        0700 Microsoft basic data  0c01 Microsoft reserved    2700 Windows RE          
        3000 ONIE boot             3001 ONIE config           3900 Plan 9              
        4100 PowerPC PReP boot     4200 Windows LDM data      4201 Windows LDM metadata
        4202 Windows Storage Spac  7501 IBM GPFS              7f00 ChromeOS kernel     
        7f01 ChromeOS root         7f02 ChromeOS reserved     8200 Linux swap          
        8300 Linux filesystem      8301 Linux reserved        8302 Linux /home         
        8303 Linux x86 root (/)    8304 Linux x86-64 root (/  8305 Linux ARM64 root (/)
        8306 Linux /srv            8307 Linux ARM32 root (/)  8400 Intel Rapid Start   
        8e00 Linux LVM             a000 Android bootloader    a001 Android bootloader 2
        a002 Android boot          a003 Android recovery      a004 Android misc        
        a005 Android metadata      a006 Android system        a007 Android cache       
        a008 Android data          a009 Android persistent    a00a Android factory     
        a00b Android fastboot/ter  a00c Android OEM           a500 FreeBSD disklabel   
        a501 FreeBSD boot          a502 FreeBSD swap          a503 FreeBSD UFS         
        a504 FreeBSD ZFS           a505 FreeBSD Vinum/RAID    a580 Midnight BSD data   
        a581 Midnight BSD boot     a582 Midnight BSD swap     a583 Midnight BSD UFS    
        a584 Midnight BSD ZFS      a585 Midnight BSD Vinum    a600 OpenBSD disklabel   
        a800 Apple UFS             a901 NetBSD swap           a902 NetBSD FFS          
        a903 NetBSD LFS            a904 NetBSD concatenated   a905 NetBSD encrypted    
        a906 NetBSD RAID           ab00 Recovery HD           af00 Apple HFS/HFS+      
        af01 Apple RAID            af02 Apple RAID offline    af03 Apple label       