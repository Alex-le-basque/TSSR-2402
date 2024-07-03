Introduction

Linux permet une gestion avancée du stockage à l'aide de Logical Volume Manager ou LVM.

Cette quête consiste à en découvrir et tester les principales fonctionalités

Les expérimentations pratiques ont été testées sur une Debian 11.3 installée dans une machine virtuelle VirtualBox 6.1 tournant sur un système hôte Ubuntu 22.04 LTS.

image

🤓 Objectifs :

✅ Comprendre le fonctionnement de LVM
✅ Explorer une installation de Debian sur LVM
✅ Expérimenter avec LVM sur un environnement de test
Sommaire

    👉 Les concepts de LVM
    👉 Installation d'un système avec LVM
        📚 Rappel sur le boot
        🔬 Exercice
        🔍 Les noms des périphériques avec LVM
    👉 Expérimentations avec LVM
        🔬 Exercice
    ☝️ Résumé
        📝 Quiz
    💪 Challenge
    🧐 Critères d'acceptation

👉 Les concepts de LVM

Commençons par une courte vidéo pour découvrir les principaux concepts de LVM.

Comme tu as pu le voir, la gestion du stockage avec LVM s'appuie sur 3 notions principales :

    Les Physical Volume ou PV
    C'est, en quelques sorte, le nom qu'utilise LVM por désigner les supports de stockage physiques. Chaque PV est en général une partition sur un disque dur classique ou un SSD.
    Les Volume Group ou VG
    Comme son nom l'indique, il s'agit d'un groupe de volumes physiques, donc d'un ensemble d'au moins un PV. Chaque VG sert à regrouper l'ensemble de l'espace de tous les PV qu'il contient en une seule réserve dont la taille est environ la somme des tailles des PV.
    Les Logical Volume ou LV
    Avec LVM, ce sont les LV que l'on utilisera à la place des partitions. Ce sont donc les LV que l'on formatera avec un système de fichier tel que ext4.

Continue ta découverte avec la lecture de la ressource suivante :

An Introduction to LVM Concepts, Terminology, and Operations

Cet article de DigitalOcean développe un peu plus les concepts de LVM et déroule un cours exemple d'utilisation.
https://www.digitalocean.com/community/tutorials/an-introduction-to-lvm-concepts-terminology-and-operations

👉 Installation d'un système avec LVM

Les programmes assistants d'installation de la plupart des distribution GNU/Linux permettent d'installer son système sur LVM.
📚 Rappel sur le boot

Une petite parenthèse s'impose pour revenir sur le démarrage (boot) d'un système d'exploitation en général et d'un linux en particulier.

Le BIOS ou le UEFI qui équipe les cartes mères ne comprends pas LVM. Aussi le bootloader et l'éventuelle partition ESP ne peuvent-elles pas être sur un LV.

GNU Grub qui est le chargeur d'amorçage habituellement utilisé pour démarrer Linux comprend LVM, néanmoins la plupart des installeurs de distribution Linux font néanmoins le choix de déporter la partition /boot sur une partition séparée, hors du PV, laissant ainsi au noyau Linux la charge d'initialiser et des monter les LV.
🔬 Exercice

Installation d'un système debian avec LVM

Sur une machine de test, par exemple une machine virtuelle, installe un système Debian dont le support d'installation peut être récupérer sur le site officiel.

Lors du choix du partitionnement, selectionne :

    Assisté - utiliser tout un disque avec LVM
    Partition /home séparée

On obtient ainsi / et /home dans 2 volumes logiques différents.

Une fois l'installation terminée, tu peux vérifier que ces 2 systèmes de fichiers sont bien sur LVM à l'aide de diverses commandes, telles que :

    lsblk qui affiche la liste des périphériques bloc et leur type donc aussi la liste des LV
    pvs, vgs et lvs qui affichent la liste courte des PV, VG et LV
    pvdisplay, vgdisplay et lvdisplay qui affichent le détail des PV, VG et LV

Les commandes LVM permettant d'interagir avec le stockage du système, elles nécessitent les droits root pour être exécutées.

Exemple

root@debian:~# lsblk 

NAME                  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT

sda                     8:0    0    8G  0 disk 

├─sda1                  8:1    0  487M  0 part /boot

├─sda2                  8:2    0    1K  0 part 

└─sda5                  8:5    0  7,5G  0 part 

  ├─debian--vg-root   254:0    0  2,8G  0 lvm  /

  ├─debian--vg-swap_1 254:1    0  976M  0 lvm  [SWAP]

  └─debian--vg-home   254:2    0  3,8G  0 lvm  /home

sr0                    11:0    1 1024M  0 rom  

root@debian:~# /sbin/pvs

  PV         VG        Fmt  Attr PSize  PFree

  /dev/sda5  debian-vg lvm2 a--  <7,52g    0 

root@debian:~# /sbin/vgs

  VG        #PV #LV #SN Attr   VSize  VFree

  debian-vg   1   3   0 wz--n- <7,52g    0 

root@debian:~# /sbin/lvs

  LV     VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert

  home   debian-vg -wi-ao----  <3,76g                                                    

  root   debian-vg -wi-ao----  <2,81g                                                    

  swap_1 debian-vg -wi-ao---- 976,00m                                                    

root@debian:~# /sbin/pvdisplay 

  --- Physical volume ---

  PV Name               /dev/sda5

  VG Name               debian-vg

  PV Size               7,52 GiB / not usable 2,00 MiB

  Allocatable           yes (but full)

  PE Size               4,00 MiB

  Total PE              1925

  Free PE               0

  Allocated PE          1925

  PV UUID               4BDQRF-inbt-ef0W-1t7R-OC08-kcpv-2lI8En

   

root@debian:~# /sbin/vgdisplay 

  --- Volume group ---

  VG Name               debian-vg

  System ID             

  Format                lvm2

  Metadata Areas        1

  Metadata Sequence No  4

  VG Access             read/write

  VG Status             resizable

  MAX LV                0

  Cur LV                3

  Open LV               3

  Max PV                0

  Cur PV                1

  Act PV                1

  VG Size               <7,52 GiB

  PE Size               4,00 MiB

  Total PE              1925

  Alloc PE / Size       1925 / <7,52 GiB

  Free  PE / Size       0 / 0   

  VG UUID               r48g5b-XSgY-ffFO-tXEQ-UtUH-jRTP-HFbrBu

   

root@debian:~# /sbin/lvdisplay 

  --- Logical volume ---

  LV Path                /dev/debian-vg/root

  LV Name                root

  VG Name                debian-vg

  LV UUID                PdrQ72-5paR-T1HN-LOUU-OHfr-mg3x-f0Ir28

  LV Write Access        read/write

  LV Creation host, time debian, 2022-10-10 10:15:54 +0200

  LV Status              available

  # open                 1

  LV Size                <2,81 GiB

  Current LE             719

  Segments               1

  Allocation             inherit

  Read ahead sectors     auto

  - currently set to     256

  Block device           254:0

   

  --- Logical volume ---

  LV Path                /dev/debian-vg/swap_1

  LV Name                swap_1

  VG Name                debian-vg

  LV UUID                0nI8rJ-iaXu-Lddz-l3QH-2IcB-t1Ac-QA3iPb

  LV Write Access        read/write

  LV Creation host, time debian, 2022-10-10 10:15:54 +0200

  LV Status              available

  # open                 2

  LV Size                976,00 MiB

  Current LE             244

  Segments               1

  Allocation             inherit

  Read ahead sectors     auto

  - currently set to     256

  Block device           254:1

   

  --- Logical volume ---

  LV Path                /dev/debian-vg/home

  LV Name                home

  VG Name                debian-vg

  LV UUID                HUayQA-CmD4-eN9X-Fmhc-ntc3-ySmK-kzuLBT

  LV Write Access        read/write

  LV Creation host, time debian, 2022-10-10 10:15:54 +0200

  LV Status              available

  # open                 1

  LV Size                <3,76 GiB

  Current LE             962

  Segments               1

  Allocation             inherit

  Read ahead sectors     auto

  - currently set to     256

  Block device           254:2

   

root@debian:~# 

Premier constat : il semble y avoir 3 partitions sur le disque

    Une pour /boot
    Une partition de 1k inutilisée
    Une partition servant de PV pour LVM

En fait il s'agit d'un schéma de partitionnement MBR avec une partition primaire pour /boot et une partition logique pour le PV /dev/sda5, et donc avec une partition étendue pour héberger les partitions logiques.

On peut d'ailleurs s'en assurer avec fdisk


root@debian:~# /sbin/fdisk -l /dev/sda

Disque /dev/sda : 8 GiB, 8589934592 octets, 16777216 secteurs

Modèle de disque : VBOX HARDDISK   

Unités : secteur de 1 × 512 = 512 octets

Taille de secteur (logique / physique) : 512 octets / 512 octets

taille d'E/S (minimale / optimale) : 512 octets / 512 octets

Type d'étiquette de disque : dos

Identifiant de disque : 0xe6c68e40


Périphérique Amorçage   Début      Fin Secteurs Taille Id Type

/dev/sda1    *           2048   999423   997376   487M 83 Linux

/dev/sda2             1001470 16775167 15773698   7,5G  5 Étendue

/dev/sda5             1001472 16775167 15773696   7,5G 8e LVM Linux

root@debian:~# 

Le type d'étiquette de disque indique dos qui est le nom d'un partitionnement MBR pour fdisk.
/dev/sda2 est bien indiquée comme une partition étendue. Elle fait d'ailleurs la même taille que dev/sda5 puisque cette dernière occupe tout l'espace disponible.

C'est l'occasion de vérifier que cette seconde partition /dev/sda5 porte bien une étiquette LVM Linux

Si besoin, la ressource suivante permet de réviser un peu le fonctionnement de MBR.

Partitions primaires et logiques sur qastack.fr

Cette discussion permet de revoir les notions de partitions primaire, étendue et logique dans le schéma de partitionnement MBR
https://qastack.fr/superuser/337146/what-are-the-differences-between-primary-and-logical-partition

En ce qui concerne LVM, on a un VG debian-vg constitué de l'unique PV /dev/sda5 dont tout l'espace disponible a été alloué aux 3 LV. En effet, le nombre de Free PE est 0.

Les 3 LV sont :

    home : monté sur /home
    root : monté sur /root
    swap_1 : qui sert d'espace de swap

Fonctionnalité intéréssante de LVM : les VG et les LV peuvent être nommés librement.
C'est donc l'occasion de rendre notre gestion du stockage facile à comprendre en choissisant judicieusement les noms utilisés.

Il est courant de préfixer (ou de suffixer) les nom de groupes de volumes avec vg et ceux des volumes logiques avec lv.
Quelques exemples :

    fastvg : Le VG avec les disques SSD du systèmes
    large-vg : Le VG avec beaucoup d'espace de stockage (et donc probablement moins rapide)
    lvlog : Le LV contenant les log (monté sur /var/log par exemple)
    db_lv : Le LV hébergeant les fichiers de la base de données

Vérifions que ces systèmes de fichiers sont bien monté au démarrage via /etc/fstab

root@debian:/dev# cat /etc/fstab 

# /etc/fstab: static file system information.

#

# Use 'blkid' to print the universally unique identifier for a

# device; this may be used with UUID= as a more robust way to name devices

# that works even if disks are added and removed. See fstab(5).

#

# systemd generates mount units based on this file, see systemd.mount(5).

# Please run 'systemctl daemon-reload' after making changes here.

#

# <file system> <mount point>   <type>  <options>       <dump>  <pass>

/dev/mapper/debian--vg-root /               ext4    errors=remount-ro 0       1

# /boot was on /dev/sda1 during installation

UUID=4390e93b-48f7-4e0c-8b6e-df33f579bada /boot           ext2    defaults        0       2

/dev/mapper/debian--vg-home /home           ext4    defaults        0       2

/dev/mapper/debian--vg-swap_1 none            swap    sw              0       0

/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0

root@debian:/dev# 

🔍 Les noms des périphériques avec LVM

Tu as sans doute remarqué que les LV sont chacun référencés à l'aide de plusieurs chemins.

lvdisplay indique par exemple /dev/<vgname>/<lvname> mais dans /etc/fstab on croise plutôt /dev/mapper/<vgname>-<lvname>.
Les fichiers représentant réellement les LV dans /dev sont en fait des pseudo-fichiers qui s'appellent dm-<numéro>

Les autres noms sont des liens symboliques et on peut donc utiliser n'importe lequel de ces noms en privilégiant sans dout ceux qui sont plus lisibles (donc pas les /dev/dm-<numéro>)

Vérifions :

root@debian:~# ls -l /dev/dm*

brw-rw---- 1 root disk 254, 0 10 oct.  11:15 /dev/dm-0

brw-rw---- 1 root disk 254, 1 10 oct.  11:15 /dev/dm-1

brw-rw---- 1 root disk 254, 2 10 oct.  11:15 /dev/dm-2

root@debian:~# ls -l /dev/mapper/

total 0

crw------- 1 root root 10, 236 10 oct.  11:15 control

lrwxrwxrwx 1 root root       7 10 oct.  11:15 debian--vg-home -> ../dm-2

lrwxrwxrwx 1 root root       7 10 oct.  11:15 debian--vg-root -> ../dm-0

lrwxrwxrwx 1 root root       7 10 oct.  11:15 debian--vg-swap_1 -> ../dm-1

root@debian:~# ls -l /dev/debian-vg/

total 0

lrwxrwxrwx 1 root root 7 10 oct.  11:15 home -> ../dm-2

lrwxrwxrwx 1 root root 7 10 oct.  11:15 root -> ../dm-0

lrwxrwxrwx 1 root root 7 10 oct.  11:15 swap_1 -> ../dm-1

root@debian:~# 

👉 Expérimentations avec LVM

LVM comprends un ensemble de commandes qui suivent en général le format <cible><operation> avec :

    <cible> = pv, vg ou lv selon ce sur quoi l'on souhaite agir
    <operation> = create, display, scan, change, remove, etc. selon l'action que l'on souhaite faire.

La documentation suivante en décrit la plupart en les regroupant selon des cas d'usage.

LVM sur Wiki ubuntu-fr

Cette page reprend les concepts de LVM ainsi qu'une liste assez complète d'opérations classiques avec LVM en détaillant les commandes pour y arriver.
https://doc.ubuntu-fr.org/lvm

🔬 Exercice

Voici quelques exemples d'expérimentations pouvant être effectuées :

    Ajouter un nouveau PV au VG.
    Créer un nouveau VG à l'aide d'un (ou de plusieurs) autre(s) PV.
    Ajouter un nouveau LV, le formater avec un système de fichier et le monter au démarrage de la machine.
    Ajouter ou convertir un LV en RAID 1
    Ajouter ou convertir un LV en RAID 0
    Faire un snapshot d'un LV (à condition d'avoir de la place dans le VG) et le monter pour avoir une copie
    Détruire un LV plus nécessaire, comme par exemple un snapshot dont on a plus l'usage.
    ...

Astuce : Dans le cas d'expérimentations sur une machine virtuelle, faire un snapshot de la VM avant de faire des manipulation permet très facilement et rapidement de revenir à l'état initial.

☝️ Résumé

LVM pour Logical Volume Manager permet sur un système Linux une gestion très flexible du stockage.
Il s'appuie sur les concepts de volume physique, de groupe de volumes et de volume logique en organisant le stockage par extends.

💪 Challenge

Le challenge consiste, en partant d'une machine debian telle que celle installée au début de cette quête, à suivre une succession d'étapes visant à effectuer des opérations classiques sur LVM.

À chaque étape, des commandes doivent être saisies pour afficher l'état du système avant les opérations et après les opérations pour pouvoir constater les modifications effectuées.

L'ensemble des commandes et leurs affichages sont à recopier dans la solution de cette quête

    Ajoute un nouveau disque à la machine et ajoute le au groupe de volume debian-vg pour au moins doubler l'espace du groupe de volume
    Créer un snapshot de LV home
    Monte le snapshot créé sur /home-snap
    Constate que /home-snap est bien une copie de /home
    On peut alors travailler sur /home-snap et y faire des modifications. En supposant qu'on a maintenant plus besoin de la copie, démonte /home-snap
    Détruit le snapshot

🧐 Critères d'acceptation

Les commandes et leurs affichages permettent bien de constater :

    L'ajout d'un PV à debian-vg et au moins le doublement des Total PE
    La création d'un snapshot du LV home
    Il y a bien création d'un dossier home-snap et montage du snapshot dans ce dossier
    L'affichage du contenu de home-snap affiche un contenu identique à /home
    L'affichage des systèmes de fichiers actuellement montés n'affiche plus /home-snap
    L'affichage des LV n'affiche plus le snapshot et le LV home n'est plus la source d'aucun snapshot
