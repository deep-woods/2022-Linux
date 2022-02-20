# <span id='top'>Install Ubuntu</span>

[[Install Guide]](#Guide)  
[[References]](#ref)

<br>

## <span id='Guide'>Install Guide</span>

[[Top]](#top)

1. Save Windows project keys.

- https://www.nirsoft.net/utils/product_cd_key_viewer.html#DownloadLinks
- Download ProduKey (In Zip file)

        ProduKey.chm
        ProduKey.exe    <----------- RUN THIS.
        readme.txt

2. Download `Ubuntu` image.

- https://ubuntu.com/download/desktop/thank-you?version=20.04.3&architecture=amd64
- Download `Ubuntu 20.04 LTS`

3. Install `Rufus`

- https://rufus.ie/en/
- Download `Rufus 3.17 (1.3 MB)`
- Run `Rufus` & select the `Ubuntu` image downloaded in step 2.

- Rufus: _Create bootable USB drives the easy way._ Rufus è un software libero "portabile" per Microsoft Windows che può essere usato per formattare e creare chiavette USB avviabili o Live USB. È sviluppato da Pete Batard di Akeo Consulting ([Wikipedia](<https://it.wikipedia.org/wiki/Rufus_(software)>)).

Rufus supporta una varietà di file .iso avviabili, incluse molte distribuzioni Linux e file .iso con l'installazione di Windows, così come i file raw .img (anche quelli compressi). Se necessario, installa un bootloader come SYSLINUX o GRUB dentro l'unità flash da rendere avviabile. Permette anche l'installazione di MS-DOS o FreeDOS dentro l'unità flash e anche la creazione di una unità Windows To Go avviabile. Supporta la formattazione delle unità flash in FAT, FAT32, NTFS, exFAT, UDF o ReFS.

Rufus può anche essere usato per calcolare gli hash MD5, SHA-1 e SHA-256 dell'immagine attualmente selezionata.

4. Burn `Ubuntu ISO` onto `Rufus`

5. Disable Auto-repoot options

- Disable `secure boot`.
  - Secure boot makes the windows start on boot.
  - `windows` > `settings` > `recovery` > click `Restart now`
  - `Troubleshoot` > `Advanced options` > `UEFI Firmware settings` > `restart`
  - In `BIOS` mode: `Security` > `Secure Boot Control` = select `Disabled`

6. Setup `Ubuntu`

- Click on `Install Ubuntu 20.04 LTS` > `Normal installation`
- `Advanced features` > `Use LVM with the new Ubuntu installation`

<br>
<br>

### <span id='ref'>References</span>

[[Top]](#top)

- Varsha Meghana (2020) Replacing pre-installed Windows with Ubuntu OS https://www.youtube.com/watch?v=BarxVQB9RN0&t=22s
