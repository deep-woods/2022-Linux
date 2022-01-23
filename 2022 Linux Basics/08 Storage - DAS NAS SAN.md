# DAS, NAS, SAN and LVM

<br>

## DAS, NAS, and SAN

- `DAS`: Direct Attacted Storage
- `NAS`: Network Attached Storage
- `SAN`: Storage Area Network

<br>

## LVM

<br>

### Physical partition vs Physical volume

<br>

<img src="https://github.com/Coding-Forest/2022-Linux/blob/main/imagesvg.gif">

image from Perez (n.d.)

`Partitions` are created from `Physical Disks` and `Physical Volumes (PVs`) are created from `Partitions`. A Physical Disk can be allocated as a single Physical Volume spanning the whole disk, or can be partitioned into multiple Physical Volumes.

<br>

- Logical Volume: Logical volumes are groups of information located on physical volumes.

A hierarchy of structures is used to manage disk storage. Each individual disk drive, called a physical volume (PV) has a name, such as /dev/hdisk0. Every physical volume in use belongs to a volume group (VG). All of the physical volumes in a volume group are divided into physical partitions (PPs) of the same size. For space-allocation purposes, each physical volume is divided into five regions (outer_edge, inner_edge, outer_middle, inner_middle and center). The number of physical partitions in each region varies, depending on the total capacity of the disk drive.

- `LVM`: Logical Volume Manager. LVM allows grouping of multiple physical volumes, which are hard disks or partitions into a volume group. From these volumegroups, you can carve out logical volumes.

**Benefits**

- Resizes the logical volume dynamically as long as there is sufficient space in the volume group.


### Configure LVM 

1, identify free disks or partitions 




then create physical volume objects for them.
<br>
<br>
<br>


- ``:
- ``:

<br>
<br>
<br>
<br>



****

- ``:
- ``:
- ``:


<br>
<br>
<br>
<br>

- ``:
- ``:
- ``:

<br>
<br>
<br>
<br>


## Exercise

1. Check if `LVM` is installed on the machine?

- `sudo lvs`:

        LV     VG         Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
        root   vagrant-vg -wi-ao----  <9.04g                                                    
        swap_1 vagrant-vg -wi-ao---- 980.00m     

<br>

- `sudo pvdisplay`

  --- Physical volume ---
  PV Name               /dev/vda1
  VG Name               vagrant-vg
  PV Size               <10.00 GiB / not usable 2.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              2559
  Free PE               0
  Allocated PE          2559
  PV UUID               ecXq0T-x7d2-Hma9-rS1w-Mi02-st3y-EqIoiW
   

<br>

2. Create a physical volume using some free disc.

        pvcreate /dev/vdb
        pvcreate /dev/vdc

        Physical volume "/dev/vdb" successfully created.
        Physical volume "/dev/vdc" successfully created.

- `apt-get install lvm2`:

<br>

3. Create a new `volume group` using the newly created `PV's`. 

Note that a volume group can have one or more physical volumes.

        sudo vgcreate forest_vg /dev/vdb /dev/vdc
        Volume group "forest_vg" successfully created

<br>

Check the PVs. 

        --- Physical volume ---
        PV Name               /dev/vda1
        VG Name               vagrant-vg
        PV Size               <10.00 GiB / not usable 2.00 MiB
        Allocatable           yes (but full)
        PE Size               4.00 MiB
        Total PE              2559
        Free PE               0
        Allocated PE          2559
        PV UUID               ecXq0T-x7d2-Hma9-rS1w-Mi02-st3y-EqIoiW

        --- Physical volume ---
        PV Name               /dev/vdb
        VG Name               forest_vg
        PV Size               1.00 GiB / not usable 4.00 MiB
        Allocatable           yes 
        PE Size               4.00 MiB
        Total PE              255
        Free PE               255
        Allocated PE          0
        PV UUID               2Uwgo1-1oal-MPkB-f28s-IdEt-qwql-O73TnS
        
        "/dev/vdc" is a new physical volume of "1.00 GiB"
        --- NEW Physical volume ---
        PV Name               /dev/vdc
        VG Name               forest_vg
        PV Size               1.00 GiB
        Allocatable           NO
        PE Size               0   
        Total PE              0
        Free PE               0
        Allocated PE          0
        PV UUID               56UW2A-6E1f-3w64-n7gm-TPzK-xqhO-4Q4ye9

<br>

4. Create a new VG using the newly created 2 PVs above.

- `sudo vgcreate forest_vg /dev/vdb /dev/vdc`

Check the VGs.

        --- Volume group ---
        VG Name               forest_vg
        System ID             
        Format                lvm2
        Metadata Areas        1
        Metadata Sequence No  1
        VG Access             read/write
        VG Status             resizable
        MAX LV                0
        Cur LV                0
        Open LV               0
        Max PV                0
        Cur PV                1
        Act PV                1
        VG Size               1020.00 MiB
        PE Size               4.00 MiB
        Total PE              255
        Alloc PE / Size       0 / 0   
        Free  PE / Size       255 / 1020.00 MiB
        VG UUID               GrkYSF-mlxr-eNOI-NdpZ-pFzK-ZPmv-k0DEvz
        
        --- Volume group ---
        VG Name               vagrant-vg
        System ID             
        Format                lvm2
        Metadata Areas        1
        Metadata Sequence No  3
        VG Access             read/write
        VG Status             resizable
        MAX LV                0
        Cur LV                2
        Open LV               2
        Max PV                0
        Cur PV                1
        Act PV                1
        VG Size               <10.00 GiB
        PE Size               4.00 MiB
        Total PE              2559
        Alloc PE / Size       2559 / <10.00 GiB
        Free  PE / Size       0 / 0   
        VG UUID               E8HKBz-6lBW-9sCH-51rQ-xu3p-J1U6-963v7c

<br>

4. Create a new LV called `forest_vol` from `forest_vg`.

- `lvcreate -L 1G -n vol1 forest_vg`
  - `-L`: linear volume. Type of logical volume. Used to make use of multiple physical volumes if available in the volume group

        sudo lvcreate -L 1G -n vol1 forest_vg
        Logical volume "vol1" created.

<br>

- Check the create `logical volume`.

        sudo lvdisplay

        sudo lvdisplay
        --- Logical volume ---
        LV Path                /dev/vagrant-vg/root
        LV Name                root
        VG Name                vagrant-vg
        LV UUID                aXeCm7-yCOC-srdD-T55P-wpHZ-c46Q-XJcX1t
        LV Write Access        read/write
        LV Creation host, time vagrant, 2021-11-21 18:46:22 +0000
        LV Status              available
        # open                 1
        LV Size                <9.04 GiB
        Current LE             2314
        Segments               1
        Allocation             inherit
        Read ahead sectors     auto
        - currently set to     256
        Block device           253:0
        
        --- Logical volume ---
        LV Path                /dev/vagrant-vg/swap_1
        LV Name                swap_1
        VG Name                vagrant-vg
        LV UUID                K00a0k-hD5J-CJZj-a3zj-CaDv-F82n-Q6ZDl0
        LV Write Access        read/write
        LV Creation host, time vagrant, 2021-11-21 18:46:22 +0000
        LV Status              available
        # open                 2
        LV Size                980.00 MiB
        Current LE             245
        Segments               1
        Allocation             inherit
        Read ahead sectors     auto
        - currently set to     256
        Block device           253:1
        
        --- Logical volume ---
        LV Path                /dev/forest_vg/vol1
        LV Name                vol1
        VG Name                forest_vg
        LV UUID                VamAB3-xAuU-OX9o-zlfa-OWLw-zBMD-2xsdzg
        LV Write Access        read/write
        LV Creation host, time forest-lp10, 2022-01-23 12:10:29 +0000
        LV Status              available
        # open                 0
        LV Size                1.00 GiB
        Current LE             256
        Segments               2
        Allocation             inherit
        Read ahead sectors     auto
        - currently set to     256
        Block device           253:2

<br>

5. Create a new LV.

        sudo lvcreate -L 1G -n data forest_vg
    
        Logical volume "data" created.

<br>

6. Create an `ext4` filesystem on this logical volume and mount it at `/mnt/media`.

        sudo mkdir /mnt/media
        sudo mkfs.ext4 /dev/mapper/forest_vg-data
        sudo mount /dev/mapper/forest_vg-data /mnt/media/

        mke2fs 1.44.1 (24-Mar-2018)
        Creating filesystem with 262144 4k blocks and 65536 inodes
        Filesystem UUID: ea7b54f5-8d35-4e6c-9e68-2ec8ab00f682
        Superblock backups stored on blocks: 
            32768, 98304, 163840, 229376

        Allocating group tables: done                            
        Writing inode tables: done                            
        Creating journal (8192 blocks): done
        Writing superblocks and filesystem accounting information: done

<br>

7. Add 800MB to the LV `data`.

        sudo lvresize -L +800M -n /dev/mapper/forest_vg-data

        Size of logical volume forest_vg/data changed from 1.00 GiB (256 extents) to <1.49 GiB (381 extents).
        Logical volume forest_vg/data successfully resized.

<br>

        sudo resize2fs /dev/mapper/forest_vg-data

        resize2fs 1.44.1 (24-Mar-2018)
        Filesystem at /dev/mapper/forest_vg-data is mounted on /mnt/media; on-line resizing required
        old_desc_blocks = 1, new_desc_blocks = 1
        The filesystem on /dev/mapper/forest_vg-data is now 390144 (4k) blocks long.

<br>

### References

  - Kodekloud.com
  - Ivan Mora Perez (n.d.) Basic guide to manage LVM opentodo.net/2012/07/basic-guide-to-manage-lvm/