# File Systems

A newly partitioned disk is just a raw disk and is not usable for the OS. So we have to create a file system to make actual use of the partition. A file system defines how data is stored on a disk.

<br>

Order

1. Partitoin disk
2. Create a file system in the partition. 
3. Mount the filesystem to start storing data. 

<br>
<br>

## Types of filesystems

<br>

The extended file system series EXT2, EXT3, and EXT4 are commonly used filesystems.

- `EXT3`: implements various features that allowed quicker startup after a shutdown. 
- `EXT4`: supports 16 TB of maximum file size and up to 1 Exabyte of volume size.

| EXT2  | EXT3   |  EXT4  |
| -- | -- | -- |
| 4TB volume   | 4TB   |  1 EB  |
| Supports compression   |  Uses journal  |  Uses journal  |
| Supports Linux permissions   |   |  Uses chksum for journal   |
| Long crash recovery   |   backward compatible  |  backward compatible |

<br>

Both are backward compatible.
- An EXT4 filesystem can be mounted as an EXT3 or EXT2 filesystem. 
 
<br>

### Persistent filesystem mount

<br>

The mount doesn't persist after the system reboots. To make this permanent, add an entry to the `/etc/fstab` file . 

    echo "<par path> <fs path> <fs type> <permission> DUMP PASS" >> /etc/fstab

<br>

|  FIELD  |  Purpose  |
| -- | -- |
| Filesystem   | Such as `/dev/vdb` to be mounted   |
| Mountpoint   | Directory to be mounted on    |
| Type   |  `ext2`, `ext3`, `ext4`  |
| Option   | RW = read-write<br>RO = Read Only   |
| Dump   |  0 = Ignore<br>1 = take backup |
| Pass   |  0 = Ignore<br>1 or 2 = FSCK filesystem check enforced.   |

<br>


## Exercise - Create a filesystem.

1. Identify a disk without a filesystem (no mountpoint)?

- `vdb` is free. 

        lsblk

        NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
        vda                    252:0    0   10G  0 disk 
        └─vda1                 252:1    0   10G  0 part 
        ├─vagrant--vg-root   253:0    0    9G  0 lvm  /
        └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
        vdb                    252:16   0    1G  0 disk 
        vdc                    252:32   0    1G  0 disk /mnt/backups

<br>

2. Figure out the type of filesystem for `vdc`

        /dev/vda1: UUID="ecXq0T-x7d2-Hma9-rS1w-Mi02-st3y-EqIoiW" TYPE="LVM2_member" PARTUUID="c0e721e6-01"
        /dev/mapper/vagrant--vg-root: UUID="bc871017-493f-41c1-b404-edf8dbf6ecef" TYPE="ext4"
        /dev/mapper/vagrant--vg-swap_1: UUID="11b93908-9eb8-4f7d-ad13-94fd79986c5a" TYPE="swap"
        /dev/vdc: UUID="ac298274-d4db-4635-82c8-505c6b864c01" TYPE="ext2"

<br>

3. Make a filesystem.

- `mkfs.<fs type> <fs path>`

        mkfs.ext4 /dev/vdb

        mke2fs 1.44.1 (24-Mar-2018)
        Creating filesystem with 262144 4k blocks and 65536 inodes
        Filesystem UUID: bc1594f7-881c-4373-a166-ea0e9c40d371
        Superblock backups stored on blocks: 
                32768, 98304, 163840, 229376

        Allocating group tables: done                            
        Writing inode tables: done                            
        Creating journal (8192 blocks): done
        Writing superblocks and filesystem accounting information: done

<br>

4. Mount it. 

- `sudo mkdir /mnt/data`
- `sudo mount /dev/vdb /mnt/data`

        NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
        vda                    252:0    0   10G  0 disk 
        └─vda1                 252:1    0   10G  0 part 
        ├─vagrant--vg-root   253:0    0    9G  0 lvm  /
        └─vagrant--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
        vdb                    252:16   0    1G  0 disk /mnt/data
        vdc                    252:32   0    1G  0 disk /mnt/backups

<br>

5. Double check.

- `mount | grep <fs path>`

        mount | grep /dev/vdb
        /dev/vdb on /mnt/data type ext4 (rw,relatime,data=ordered)
        
<fs> is mount on <this path>. Filesystem type is <this>.

<br>

- `df -hP | grep /dev/vdb`

        /dev/vdb                      976M  2.6M  907M   1% /mnt/data
