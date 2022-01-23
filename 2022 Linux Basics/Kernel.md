# Kernel
        
The core interface between a computer's hardware and its processes; The kernel is the central module of an operating system (OS). It is the part of the operating system that loads first, and it remains in main memory.

- Hardware: memory, CPU, devices
- Some hardware-related commands:

  - `uname`: get name and information about current kernel

  - `dmesg`: print or control the kernel ring buffer

  - `udevadm`: udev management tool. udevadm expects a command and command specific options. It controls the runtime behavior of systemd-udevd, requests kernel events, manages the event queue, and provides simple debugging mechanisms.

        udevadm info --query=path --name=/dev/sda5

        udevadm monitor

        monitor will print the received events for:
        UDEV - the event which udev sends out after rule processing
        KERNEL - the kernel uevent

  - `lspci`: list PCI. PCI stands for Peripheral Component Interconnect and is an industry standard bus for attaching peripheral devices to a computer. 

        lspci
        00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (rev 01)
        00:01.0 PCI bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX AGP bridge (rev 01)
        00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 08)
        00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
        00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 08)
        00:07.7 System peripheral: VMware Virtual Machine Communication Interface (rev 10)
        00:0f.0 VGA compatible controller: VMware SVGA II Adapter
        00:10.0 SCSI storage controller: Broadcom / LSI 53c1030 PCI-X Fusion-MPT Dual Ultra320 SCSI (rev 01)
        00:11.0 PCI bridge: VMware PCI bridge (rev 02)

        ...

        00:18.7 PCI bridge: VMware PCI Express Root Port (rev 01)
        02:00.0 USB controller: VMware USB1.1 UHCI Controller
        02:01.0 Ethernet controller: Intel Corporation 82545EM Gigabit Ethernet Controller (Copper) (rev 01)
        02:02.0 Multimedia audio controller: Ensoniq ES1371/ES1373 / Creative Labs CT2518 (rev 02)
        02:03.0 USB controller: VMware USB2 EHCI Controller
        02:04.0 SATA controller: VMware SATA AHCI controller

        - Process: application running...

  - `lsblk`: lists information about all available or the specified block devices.

        NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
        loop0    7:0    0  55.5M  1 loop /snap/core18/2284
        loop1    7:1    0 247.9M  1 loop /snap/gnome-3-38-2004/87

        ...

        loop13   7:13   0  61.9M  1 loop /snap/core20/1270
        sda      8:0    0    20G  0 disk 
        ├─sda1   8:1    0   512M  0 part /boot/efi
        ├─sda2   8:2    0     1K  0 part 
        └─sda5   8:5    0  19.5G  0 part /
        sr0     11:0    1  1024M  0 rom  

  - `lshw`: lshw is a small tool to extract detailed information on the hardware configuration of the machine. It can report exact memory configuration,  firmware  version,  mainboard configuration, CPU version and speed, cache configuration, bus speed, etc. on DMI-capable x86 or IA-64 systems and on some PowerPC machines  (PowerMac  G4  is  known  to work).

        WARNING: you should run this program as super-user.
        ubuntu                      
            description: Computer
            width: 64 bits
            capabilities: smp vsyscall32
        *-core
            description: Motherboard
            physical id: 0
            *-memory
                description: System memory
                physical id: 0
                size: 4GiB
            *-cpu:0
                product: Intel(R) Core(TM) i7-10750H CPU @ 2.60GHz
                vendor: Intel Corp.
                physical id: 1
                bus info: cpu@0
                width: 64 bits

                ...

            *-pci
                description: Host bridge
                product: 440BX/ZX/DX - 82443BX/ZX/DX Host bridge
                vendor: Intel Corporation
                physical id: 100
                bus info: pci@0000:00:00.0
                version: 01
                width: 32 bits
                clock: 33MHz
                configuration: driver=agpgart-intel
                resources: irq:0

  - `lsmem`: list the ranges of available memory with their online status

        RANGE                                 SIZE  STATE REMOVABLE BLOCK
        0x0000000000000000-0x00000000bfffffff   3G online       yes  0-23
        0x0000000100000000-0x000000013fffffff   1G online       yes 32-39

        Memory block size:       128M
        Total online memory:       4G
        Total offline memory:      0B

  - `free -m`: `-m` for MB. `-g` for GB.

        total        used        free      shared  buff/cache   available
        Mem:           3893        2577         316          44         999        1052
        Swap:           923         119         803


- `sda`: Small Computer System Interface disk == SCSI Disk. First drive (`a`) of SCSI disk (`sd`).
- type disk: the whole physical disk
- type partition: a reusable disk space carved out of the physical disk.


<br>

Kerne's responsibilities

- Memory management: keep track of memory usage and storage location
- Process management
- Device drivers: mediary/interpreter between hardware and processes
- System calls and security: receive requests for service from the processes.

Characteristics 

- Monolithic
- Modular

<br>

**Kernel Version**


      uname -r
      5.13.0-27-generic

[Kernel version].[Major verison].[Minor version]-[Patch release]-[Distro specific info]

<br>

**Kernel's memory management**

|  Kernel space  | User space |
| -- | -- |
|  Kernel  |  Applications  |
|  Device drivers  |  Programmes  |
| Kernel code<br>Kernel extentions<br>Device drivers| C<br>Java<br>Python<br>Ruby<br>Docker containers|

<br>

### References

  - https://kodekloud.com/
  - https://kernel.org
  - https://www.asciiart.eu/
 