---
description: mount and umount respectivly mount or unmount devices or other computers from your directory structure
usage: |-
  ````bash 
  mount /dev/sdx
week: "1"
tags:
  - CS3
  - linuxTuning
  - command
---
## extra info:

Mounten van een FAT32 USB stick of ext2 USB stick :
```bash
mount -t vfat /dev/sdb1 /media/disk  
mount -t ext2 /dev/sdb1 /media/disk
```
Mounten van een CD-ROM of dvd:
```bash
mount -rt iso9660 /dev/cdrom /media/cdrom
```
Mounten van een windows ntfs partitie:
```bash
mkdir /windows  
mount -rt ntfs /dev/hda1 /windows
```
Opm: Mounten van een gecrashte NTFS schijf lukt voorlopig niet.  
Mounten van een loop device (bv iso of initrd). Dit zijn bestanden die zelf een filesystem  
structuur bevatten. Typische bestanden zijn iso files, het initrd filesystem dat een tijdelijk  
filesystem in RAM aanmaakt en img bestanden.  
Linux maakt hierbij een speciaal bestand een loop device node (/dev/loop0)

