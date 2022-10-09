# **MOREL Loïc 3ICS**

## 1. Table des matières

- [**MOREL Loïc 3ICS**](#morel-loïc-3ics)
  - [1. Table des matières](#1-table-des-matières)
  - [Exercice 1. Disques et partitions](#exercice-1-disques-et-partitions)
    - [1. Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement alloués ; puis démarrez la VM](#1-dans-linterface-de-configuration-de-votre-vm-créez-un-second-disque-dur-de-5-go-dynamiquement-alloués--puis-démarrez-la-vm)
    - [2. Vérifiez que ce nouveau disque dur est bien détecté par le système](#2-vérifiez-que-ce-nouveau-disque-dur-est-bien-détecté-par-le-système)
    - [3. Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7)](#3-partitionnez-ce-disque-en-utilisant-fdisk--créez-une-première-partition-de-2-go-de-type-linux-n83-et-une-seconde-partition-de-3-go-en-ntfs-n7)
      - [Création de la première partition de 2 Go](#création-de-la-première-partition-de-2-go)
      - [Création de la première partition de 3 Go](#création-de-la-première-partition-de-3-go)
    - [4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions ( pensez à consulter le manuel !)](#4-a-ce-stade-les-partitions-ont-été-créées-mais-elles-nont-pas-été-formatées-avec-leur-système-de-fichiers-a-laide-de-la-commande-mkfs-formatez-vos-deux-partitions--pensez-à-consulter-le-manuel-)
      - [Formatage de la première partition en ext4](#formatage-de-la-première-partition-en-ext4)
      - [Formatage de la seconde partition en NTFS](#formatage-de-la-seconde-partition-en-ntfs)
    - [5. Pourquoi la commande df -T, qui aﬀiche le type de système de fichier des partitions, ne fonctionne-t- elle pas sur notre disque ?](#5-pourquoi-la-commande-df--t-qui-aﬀiche-le-type-de-système-de-fichier-des-partitions-ne-fonctionne-t--elle-pas-sur-notre-disque-)
    - [6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)](#6-faites-en-sorte-que-les-deux-partitions-créées-soient-montées-automatiquement-au-démarrage-de-la-machine-respectivement-dans-les-points-de-montage-data-et-win-vous-pourrez-vous-passer-des-uuid-en-raison-de-limpossibilité-deffectuer-des-copier-coller)
      - [Création des points de montage](#création-des-points-de-montage)
      - [Ajout des partitions dans le fichier /etc/fstab](#ajout-des-partitions-dans-le-fichier-etcfstab)
    - [7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration](#7-utilisez-la-commande-mount-puis-redémarrez-votre-vm-pour-valider-la-configuration)
    - [8. Montez votre clé USB dans la VM](#8-montez-votre-clé-usb-dans-la-vm)
    - [9. Créez un dossier partagé entre votre VM et votre système hôte (rem. il peut être nécessaire d’installer les Additions invité de VirtualBox)](#9-créez-un-dossier-partagé-entre-votre-vm-et-votre-système-hôte-rem-il-peut-être-nécessaire-dinstaller-les-additions-invité-de-virtualbox)
  - [Exercice 2. Partitionnement LVM](#exercice-2-partitionnement-lvm)
    - [1. On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab](#1-on-va-réutiliser-le-disque-de-5-gio-de-lexercice-précédent-commencez-par-démonter-les-systèmes-de-fichiers-montés-dans-data-et-win-sils-sont-encore-montés-et-supprimez-les-lignes-correspondantes-du-fichier-etcfstab)
    - [2. Supprimez les deux partitions du disque, et créez une partition unique de type LVM](#2-supprimez-les-deux-partitions-du-disque-et-créez-une-partition-unique-de-type-lvm)
    - [3. A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en utilisant la commande pvdisplay.](#3-a-laide-de-la-commande-pvcreate-créez-un-volume-physique-lvm-validez-quil-est-bien-créé-en-utilisant-la-commande-pvdisplay)
    - [4. A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay.](#4-a-laide-de-la-commande-vgcreate-créez-un-groupe-de-volumes-qui-pour-linstant-ne-contiendra-que-le-volume-physique-créé-à-létape-précédente-vérifiez-à-laide-de-la-commande-vgdisplay)
    - [5. Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.](#5-créez-un-volume-logique-appelé-lvdata-occupant-lintégralité-de-lespace-disque-disponible)
    - [6. Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data.](#6-dans-ce-volume-logique-créez-une-partition-que-vous-formaterez-en-ext4-puis-procédez-comme-dans-lexercice-1-pour-quelle-soit-montée-automatiquement-au-démarrage-de-la-machine-dans-data)
    - [7. Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.](#7-eteignez-la-vm-pour-ajouter-un-second-disque-peu-importe-la-taille-pour-cet-exercice-redémarrez-la-vm-vérifiez-que-le-disque-est-bien-présent-puis-répétez-les-questions-2-et-3-sur-ce-nouveau-disque)
    - [8. Utilisez la commande vgextend <nom_vg> <nom_pv> pour ajouter le nouveau disque au groupe de volumes](#8-utilisez-la-commande-vgextend-nom_vg-nom_pv-pour-ajouter-le-nouveau-disque-au-groupe-de-volumes)
    - [9. Utilisez la commande lvresize (ou lvextend) pour agrandir le volume logique. Enfin, il ne faut pas oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.](#9-utilisez-la-commande-lvresize-ou-lvextend-pour-agrandir-le-volume-logique-enfin-il-ne-faut-pas-oublier-de-redimensionner-le-système-de-fichiers-à-laide-de-la-commande-resize2fs)

## Exercice 1. Disques et partitions

### 1. Dans l’interface de configuration de votre VM, créez un second disque dur, de 5 Go dynamiquement alloués ; puis démarrez la VM

![image](./media/img_tp5_1.png)

### 2. Vérifiez que ce nouveau disque dur est bien détecté par le système

```bash
sudo fdisk -l
```

```Text
Disk /dev/sdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Disk model: Virtual disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

### 3. Partitionnez ce disque en utilisant fdisk : créez une première partition de 2 Go de type Linux (n°83), et une seconde partition de 3 Go en NTFS (n°7)

#### Création de la première partition de 2 Go


```Console
sudo fdisk /dev/sdb
```

```Text
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-10485759, default 10485759): +2G
```

#### Création de la première partition de 3 Go

```Text
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2): 2
First sector (4196352-10485759, default 4196352): 4196352
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4096-10485759, default 10485759): 10485759
t
Partition number (1,2, default 2): 2
Hex code or alias (type L to list all): 7

```

```Text
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```


### 4. A ce stade, les partitions ont été créées, mais elles n’ont pas été formatées avec leur système de fichiers. A l’aide de la commande mkfs, formatez vos deux partitions ( pensez à consulter le manuel !)

#### Formatage de la première partition en ext4

```Console
sudo mkfs.ext4 /dev/sdb1

mke2fs 1.46.5 (30-Dec-2021)
Discarding device blocks: done
Creating filesystem with 524288 4k blocks and 131072 inodes
Filesystem UUID: 2c7626b2-cdfb-4255-ae28-fe59506210b9
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```


#### Formatage de la seconde partition en NTFS

```Console
sudo mkfs.ntfs /dev/sdb2

Cluster size has been automatically set to 4096 bytes.
Initializing device with zeroes: 100% - Done.
Creating NTFS volume structures.
mkntfs completed successfully. Have a nice day.
```


### 5. Pourquoi la commande df -T, qui aﬀiche le type de système de fichier des partitions, ne fonctionne-t- elle pas sur notre disque ?

```Console
df -T
```

```Text
Filesystem              Type  1K-blocks    Used Available Use% Mounted on
tmpfs                   tmpfs    150648    1360    149288   1% /run
/dev/mapper/sysvg-root  xfs    12572672 6226612   6346060  50% /
tmpfs                   tmpfs    753220       0    753220   0% /dev/shm
tmpfs                   tmpfs      5120       0      5120   0% /run/lock
/dev/mapper/sysvg-opt   xfs     2086912   47604   2039308   3% /opt
/dev/sda2               xfs     1038336  294380    743956  29% /boot
/dev/sda1               vfat    1046512    5364   1041148   1% /boot/efi
/dev/mapper/sysvg-var   xfs     4184064  941012   3243052  23% /var
/dev/mapper/sysvg-home  xfs     4184064   63500   4120564   2% /home
/dev/mapper/sysvg-audit xfs     4184064   62248   4121816   2% /var/audit
/dev/mapper/sysvg-log   xfs     4184064  248184   3935880   6% /var/log
/dev/mapper/sysvg-tmp   xfs     3135488   55008   3080480   2% /tmp
tmpfs                   tmpfs    150644       4    150640   1% /run/user/1000
```

On ne peut pas voir le type de système de fichier des partitions car elles ne sont pas montées.

### 6. Faites en sorte que les deux partitions créées soient montées automatiquement au démarrage de la machine, respectivement dans les points de montage /data et /win (vous pourrez vous passer des UUID en raison de l’impossibilité d’effectuer des copier-coller)

#### Création des points de montage

```Console
sudo mkdir /data
sudo mkdir /win
```

#### Ajout des partitions dans le fichier /etc/fstab

```Console
sudo nano /etc/fstab
```

```Text
/dev/sdb1 /data ext4 defaults 0 0
/dev/sdb2 /win ntfs-3g defaults 0 0
```

### 7. Utilisez la commande mount puis redémarrez votre VM pour valider la configuration

```Console
sudo mount /dev/sdb1 /data
sudo mount /dev/sdb2 /win
```

```

```Text
/dev/sdb1               ext4      1992552      24   1871288   1% /data
/dev/sdb2               fuseblk   3144700   16264   3128436   1% /win
```

```Console
sudo reboot
```

```Console
df -T
```

```Text
/dev/sdb1               ext4      1992552      24   1871288   1% /data
/dev/sdb2               fuseblk   3144700   16264   3128436   1% /win
```


### 8. Montez votre clé USB dans la VM

### 9. Créez un dossier partagé entre votre VM et votre système hôte (rem. il peut être nécessaire d’installer les Additions invité de VirtualBox)


## Exercice 2. Partitionnement LVM
Dans cet exercice, nous allons aborder le partitionnement LVM, beaucoup plus flexible pour manipuler les disques et les partitions.
### 1. On va réutiliser le disque de 5 Gio de l’exercice précédent. Commencez par démonter les systèmes de fichiers montés dans /data et /win s’ils sont encore montés, et supprimez les lignes correspondantes du fichier /etc/fstab

```Console
sudo umount /data
sudo umount /win
```

```Console
sudo nano /etc/fstab
```

```Text
#/dev/sdb1 /data ext4 defaults 0 0
#/dev/sdb2 /win ntfs-3g defaults 0 0
```


### 2. Supprimez les deux partitions du disque, et créez une partition unique de type LVM
 La création d’une partition LVM n’est pas indispensable, mais vivement recommandée quand on utilise LVM sur un disque entier. En effet, elle permet d’indiquer à d’autres OS ou logiciels de gestion de disques (qui ne reconnaissent pas forcément le format LVM) qu’il y a des données sur ce disque.
 Attention à ne pas supprimer la partition système !

```Console
sudo fdisk /dev/sdb
```

```Text
Command (m for help): d
Selected partition 1
Partition 1 has been deleted.
Command (m for help): d
Selected partition 2
Partition 2 has been deleted.
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G,T,P} (2048-10485759, default 10485759): +5G
Partition 1 of type Linux and of size 5 GiB is set
Command (m for help): t
Selected partition 1
Hex code (type L to list codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```


### 3. A l’aide de la commande pvcreate, créez un volume physique LVM. Validez qu’il est bien créé, en utilisant la commande pvdisplay.
 Toutes les commandes concernant les volumes physiques commencent par pv. Celles concernant
les groupes de volumes commencent par vg, et celles concernant les volumes logiques par lv.

```Console
sudo pvcreate /dev/sdb1
```

```Text
  Physical volume "/dev/sdb1" successfully created.
```

```Console
sudo pvdisplay
```

```Text
  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               sysvg
  PV Size               <38.00 GiB / not usable 2.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              9727
  Free PE               1279
  Allocated PE          8448
  PV UUID               wQIWMI-3vhz-xBTe-TItJ-wtvS-AE3I-3wZGd4

  "/dev/sdb1" is a new physical volume of "<5.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name
  PV Size               <5.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               wt0OUG-ZETN-JdtK-4kQA-Tc0f-nKIx-YcW5lu
```


### 4. A l’aide de la commande vgcreate, créez un groupe de volumes, qui pour l’instant ne contiendra que le volume physique créé à l’étape précédente. Vérifiez à l’aide de la commande vgdisplay.
 Par convention, on nomme généralement les groupes de volumes vgxx (où xx représente l’indice du groupe de volume, en commençant par 00, puis 01...)

```Console
sudo vgcreate vg00 /dev/sdb1
```

```Text
  Volume group "vg00" successfully created
```

```Console
sudo vgdisplay
```

```Text
  --- Volume group ---
  VG Name               vg00
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
  VG Size               <5.00 GiB
  PE Size               4.00 MiB
  Total PE              1279
  Alloc PE / Size       0 / 0
  Free  PE / Size       1279 / <5.00 GiB
  VG UUID               f6PC8W-6R8R-dsEO-pkxl-PKDZ-bdBF-nJmyOg

  --- Volume group ---
  VG Name               sysvg
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  8
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                7
  Open LV               7
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <38.00 GiB
  PE Size               4.00 MiB
  Total PE              9727
  Alloc PE / Size       8448 / 33.00 GiB
  Free  PE / Size       1279 / <5.00 GiB
  VG UUID               UC41XH-YKj4-8g4u-MZkc-a1we-7W27-g9xMRV
```

### 5. Créez un volume logique appelé lvData occupant l’intégralité de l’espace disque disponible.
 On peut renseigner la taille d’un volume logique soit de manière absolue avec l’option -L (par exemple -L 10G pour créer un volume de 10 Gio), soit de manière relative avec l’option -l : -l 60%VG pour utiliser 60% de l’espace total du groupe de volumes, ou encore -l 100%FREE pour utiliser la totalité de l’espace libre.

```Console
sudo lvcreate -l 100%FREE -n lvData vg00
```

```Text
  Logical volume "lvData" created.
```

```Console
sudo lvdisplay
```

```Text
  --- Logical volume ---
  LV Path                /dev/vg00/lvData
  LV Name                lvData
  VG Name                vg00
  LV UUID                U6up0L-Y8qc-Q6gy-8i2n-MsjK-gPmK-0GMcCT
  LV Write Access        read/write
  LV Creation host, time SRVDHCP.tpadmin.local, 2022-10-06 09:14:06 +0000
  LV Status              available
  # open                 0
  LV Size                <5.00 GiB
  Current LE             1279
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:7

  --- Logical volume ---
  LV Path                /dev/sysvg/root
  LV Name                root
  VG Name                sysvg
  LV UUID                f4kIPt-lNf0-CJl7-C8fl-c3kX-u0HU-VYhZXU
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:26 +0000
  LV Status              available
  # open                 1
  LV Size                12.00 GiB
  Current LE             3072
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Logical volume ---
  LV Path                /dev/sysvg/home
  LV Name                home
  VG Name                sysvg
  LV UUID                ZoabWh-rGlq-jvvx-SmyA-sdzj-SQwE-lam0Ac
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:26 +0000
  LV Status              available
  # open                 1
  LV Size                4.00 GiB
  Current LE             1024
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/sysvg/opt
  LV Name                opt
  VG Name                sysvg
  LV UUID                ofJq2u-QKLe-zmYG-ZiPK-DR6N-g9xL-Mvqb05
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:27 +0000
  LV Status              available
  # open                 1
  LV Size                2.00 GiB
  Current LE             512
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:2

  --- Logical volume ---
  LV Path                /dev/sysvg/tmp
  LV Name                tmp
  VG Name                sysvg
  LV UUID                Wm5zSw-juG8-k67h-t3Hl-G3Mm-L4Ft-neypj3
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:28 +0000
  LV Status              available
  # open                 1
  LV Size                3.00 GiB
  Current LE             768
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:3

  --- Logical volume ---
  LV Path                /dev/sysvg/var
  LV Name                var
  VG Name                sysvg
  LV UUID                3a9EQC-I946-gFwP-wGuZ-iFK2-9DYi-ntPGjA
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:29 +0000
  LV Status              available
  # open                 1
  LV Size                4.00 GiB
  Current LE             1024
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:4

  --- Logical volume ---
  LV Path                /dev/sysvg/log
  LV Name                log
  VG Name                sysvg
  LV UUID                SsUUCS-btwN-FR2f-YRgC-e6HB-fav4-7Wy9Ee
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:30 +0000
  LV Status              available
  # open                 1
  LV Size                4.00 GiB
  Current LE             1024
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:5

  --- Logical volume ---
  LV Path                /dev/sysvg/audit
  LV Name                audit
  VG Name                sysvg
  LV UUID                TxkyFM-3YiZ-AzO8-O2O8-uxwP-kB2x-mmKWwB
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2022-05-27 08:32:30 +0000
  LV Status              available
  # open                 1
  LV Size                4.00 GiB
  Current LE             1024
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:6
```

### 6. Dans ce volume logique, créez une partition que vous formaterez en ext4, puis procédez comme dans l’exercice 1 pour qu’elle soit montée automatiquement, au démarrage de la machine, dans /data.
 A ce stade, l’utilité de LVM peut paraître limitée. Il trouve tout son intérêt quand on veut par exemple agrandir une partition à l’aide d’un nouveau disque.

```Console
sudo mkfs.ext4 /dev/vg00/lvData

mke2fs 1.46.5 (30-Dec-2021)
Found a dos partition table in /dev/vg00/lvData
Proceed anyway? (y,N) y
Discarding device blocks: done
Creating filesystem with 1309696 4k blocks and 327680 inodes
Filesystem UUID: 75061516-7c89-47d0-957b-93912fe354fd
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

sudo mkdir /data

sudo mount /dev/vg00/lvData /data
/dev/mapper/vg00-lvData ext4    5070496      24   4792152   1% /data
```

```Console
nano /etc/fstab
```

```Text
/dev/mapper/vg00-lvData /data ext4 defaults 0 0
```

```Console
sudo mount -a
```

```Console
sudo reboot
/dev/mapper/vg00-lvData ext4    5070496      24   4792152   1% /data
```


### 7. Eteignez la VM pour ajouter un second disque (peu importe la taille pour cet exercice). Redémarrez la VM, vérifiez que le disque est bien présent. Puis, répétez les questions 2 et 3 sur ce nouveau disque.

```Console	
sudo fdisk -l

Disk /dev/sdc: 16 GiB, 17179869184 bytes, 33554432 sectors
Disk model: Virtual disk
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

```Console
sudo pvcreate /dev/sdc
```

```Text
  Physical volume "/dev/sdb1" successfully created.
```

```Console
sudo pvdisplay
```

```Text
--- Physical volume ---
  PV Name               /dev/sdb1
  VG Name               vg00
  PV Size               <5.00 GiB / not usable 3.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              1279
  Free PE               0
  Allocated PE          1279
  PV UUID               wt0OUG-ZETN-JdtK-4kQA-Tc0f-nKIx-YcW5lu

  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               sysvg
  PV Size               <38.00 GiB / not usable 2.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              9727
  Free PE               1279
  Allocated PE          8448
  PV UUID               wQIWMI-3vhz-xBTe-TItJ-wtvS-AE3I-3wZGd4

  "/dev/sdc" is a new physical volume of "16.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdc
  VG Name
  PV Size               16.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               K3fWvU-RWvk-ibFE-dWeX-EoxT-5ZWv-chYLTz
```

### 8. Utilisez la commande vgextend <nom_vg> <nom_pv> pour ajouter le nouveau disque au groupe de volumes

```Console
sudo vgextend vg00 /dev/sdc

  Volume group "vg00" successfully extended
```

### 9. Utilisez la commande lvresize (ou lvextend) pour agrandir le volume logique. Enfin, il ne faut pas oublier de redimensionner le système de fichiers à l’aide de la commande resize2fs.
 Il est possible d’aller beaucoup plus loin avec LVM, par exemple en créant des volumes par bandes (l’équivalent du RAID 0) ou du mirroring (RAID 1). Le but de cet exercice n’était que de présenter les fonctionnalités de base.

```Console
sudo lvresize -L +10G /dev/vg00/lvData

Size of logical volume vg00/lvData changed from <5.00 GiB (1279 extents) to <15.00 GiB (3839 extents).
  Logical volume vg00/lvData successfully resized.
```

```Console
sudo resize2fs /dev/vg00/lvData


resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/vg00/lvData is mounted on /data; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 2
The filesystem on /dev/vg00/lvData is now 3931136 (4k) blocks long.
```

```Console
df -h
/dev/mapper/vg00-lvData   15G   24K   15G   1% /data
```
