Introduction

Le RAID (pour Redundant Array of Inexpensive Disks) est un système avancé de gestion du stockage utilisé dans les entreprises qui mettent en place une politique de sécurisation de leurs données.
Le RAID est un système qui utilise plusieurs disques durs physiques pour une gestion virtuelle des données.
Il existe 2 sortes de RAID : le RAID matériel qui utilise des cartes contrôleurs qui se gère directement au niveau de la couche physique, et le RAID logiciel qui se créer et se gère à partir d'un système d'exploitation.
Plusieurs niveaux de RAID existent, de même que plusieurs combinaisons. Suivant le niveau de RAID utilisé, on améliore la performance, la fiabilité et/ou la capacité de stockage des données.
Dans cette quête tu vas voir une rapide définition des niveaux de RAID les plus utilisés avant d’expérimenter du RAID 1.

La quête suivante n'est pas un prérequis. Néanmoins, la manière d'opérer dans partie Simulation d'une panne reprend le même principe pour Linux.

🤓 Objectifs :

✅ Comprendre les différents niveaux de RAID classiques
✅ Manipuler des disques pour avoir du RAID 1
✅ Réparer un RAID 1 défectueux
✅ Simuler une panne de disque dur en ligne de commande
Sommaire

    📌 Glossaire
    📌 Les niveaux de RAID les plus courant
    👉 Mise en œuvre
        🔧 Prérequis
            ⌨️ L'outil mdadm
        🔬 Création d'un RAID 1
            Partitionnement des disques
            Création du RAID
            Vérification
            Formatage du RAID
            Montage du RAID
            Verrouillage du nom md0 de la partition RAID
        🔧 Test du bon fonctionnement du RAID 1
        🔧 Simulation d'une panne
            Vérification de la tolérance aux pannes
            Reconstruction du RAID
    📝 Quiz
    ☝️ Résumé
    💪 Challenge
    🧐 Critères d'acceptation

📌 Glossaire

Des termes spécifiques sont utilisés dans le contexte du stockage. Tu as ci-dessous quelques définitions :

    Un disque dur (HDD) ou un SSD est un périphérique de stockage de données aussi appelé périphérique de blocs (block device).

    Un bloc est une unité de stockage de données de petite taille (de 512 octets ou 4096 octets). Les données sont souvent stockées en plusieurs blocs sur un disque dur.

    Une grappe (ou cluster ou array) est une collection de disques durs reliés ensemble.

    Un calcul de parité est une technique utilisée pour protéger les données en cas de défaillance d'un disque dur. C'est une information supplémentaire stockée sur un ou plusieurs disques durs qui peut être utilisée pour reconstruire les données en cas de défaillance d'un disque.

📌 Les niveaux de RAID les plus courant

De nombreux niveaux de RAID existent, certains sont plus utilisés que d'autres. En voici donc quelques un :

    Le RAID 0 (entrelaçage par bande ou striping) :
    Les données sont divisées en blocs et stockées sur différents disques. Cela améliore les performances en raison de la lecture et de l'écriture simultanées sur plusieurs disques, mais ne fournit pas de redondance. Si l'un des disques est en panne, toutes les données sont perdues.

schéma du RAID 0 stripping

    Le RAID 1 (disques mirroirs ou mirroring) :
    Les données sont stockées en double sur deux disques. Si l'un des disques est en panne, les données peuvent être lues à partir de l'autre disque. Ce niveau de RAID fournit une très haute fiabilité, mais ne fournit pas un gain de performance significatif.

schéma du RAID 1 mirroring

    Le RAID 5 :
    Ce niveau implique l'utilisation d'au moins 3 disques. Les données sont divisées en blocs et stockées sur différents disques, tandis que des informations de parité sont stockées (de manière tournante) sur un autre disque. Si l'un des disques dysfonctionne, les informations de parité peuvent être utilisées pour reconstruire les données perdue sur un nouveau disque.

schéma du RAID 5

    Le RAID 1+0 ou RAID 10 :
    Cette combinaison de RAID 1 et 0, appelé plus communément RAID 10 combine le striping et le mirroring. Les données sont d'abord mises en miroir sur deux groupes de disques (RAID 1), puis divisées en blocs et stockées sur plusieurs disques dans chaque groupe (RAID 0). Ce niveau de RAID fournit à la fois de hautes performances et une fiabilité élevée.
    En effet, en cas de défaillance d'un disque dans un groupe, les données peuvent être lues à partir du miroir grâce au RAID 1. De plus, le RAID 0 offre, avec la duplication des blocs de très bonnes performances de lecture.

schéma du RAID 10

Le RAID sur WikipédiA

Pour en apprendre un peu plus sur le RAID, tu peux consulter cette page WikipédiA
https://fr.wikipedia.org/wiki/RAID_(informatique)

La vidéo suivante constitue une bonne illustration des différents types de RAID.

👉 Mise en œuvre
🔧 Prérequis

Pour pouvoir expérimenter sereinement, il est préférable de travailler sur une machine dédiée au test.
Il est ainsi possible de la redémarrer, reconfigurer voir casser sans impact sur les utilisateurs.
Une stratégie courante est de s'appuyer sur des machines virtuelles avec des disques virtuels.

Les expérimentations pratiques ont été testées sur une distribution Linux Ubuntu 22.04 LTS installée dans une machine virtuelle VirtualBox 7 tournant sur un système hôte Ubuntu 22.04 LTS.
Elles peuvent être reproduites avec d'autres distributions Linux, sur d'autres environnement, mais des différences peuvent alors apparaître.

La machine de test est une VM avec la configuration suivante :

    OS : Linux Ubuntu 22.04 LTS
    Disque 1 : pour le système
    3 disques supplémentaires de 5 Go chacun pour les manipulations sur le RAID. Tu peux changer la taille mais fait en sorte qu'elle soit identique pour tous les disques.
    Attention à bien cocher la case pour que les disques soit branchable à chaud (ou hotplug).

Pour du RAID 1, les disques peuvent avoir des tailles différentes, mais la taille du plus petit des disques limite la taille de la grappe.
Par exemple, pour du RAID 1 avec un disque de 100 Go et un disque de 500 Go, la taille du volume du RAID sera de 100 Go.

⌨️ L'outil mdadm

Pour administrer le RAID logiciel sous GNU/Linux, on utilise l'outil mdadm. S'il n'est pas installé sur ton système, exécute la commande apt install mdadm pour l'installer.

Mdadm

La page Wikipedia de cet outil de gestion.
https://en.wikipedia.org/wiki/Mdadm

🔬 Création d'un RAID 1

L'objectif ici est de mettre en place un RAID 1, soit du mirroring entre les 2 disques. Les données sont copiées simultanément sur deux disques. Ainsi en cas de panne d'un des disques, les données sont toujours disponibles sur le disque restant.

Une fois la grappe RAID créée, elle apparaît sous la forme d'un unique périphérique de blocs supplémentaire. C'est ce qui rend le RAID transparent pour les applications qui continuent à manipuler l'équivalent d'un disque.

Partitionnement des disques

Avec la commande lsblk on peut voir les 3 disques qui vont servir au RAID :

wilder@Ubuntu:~$ lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS


[...]

                                 /

sdb      8:16   0     5G  0 disk 

sdc      8:32   0     5G  0 disk 

sdd      8:48   0     5G  0 disk 


[...]

    On voit bien que les 3 disques font 5 Go chacun

Avec la commande fdisk -l on a plus de détails :

wilder@Ubuntu:~$ sudo fdisk -l


[...]


Disque /dev/sdb : 5 GiB, 5368709120 octets, 10485760 secteurs

Disk model: VBOX HARDDISK   

Unités : secteur de 1 × 512 = 512 octets

Taille de secteur (logique / physique) : 512 octets / 512 octets

taille d'E/S (minimale / optimale) : 512 octets / 512 octets



Disque /dev/sdc : 5 GiB, 5368709120 octets, 10485760 secteurs

Disk model: VBOX HARDDISK   

Unités : secteur de 1 × 512 = 512 octets

Taille de secteur (logique / physique) : 512 octets / 512 octets

taille d'E/S (minimale / optimale) : 512 octets / 512 octets



Disque /dev/sdd : 5 GiB, 5368709120 octets, 10485760 secteurs

Disk model: VBOX HARDDISK   

Unités : secteur de 1 × 512 = 512 octets

Taille de secteur (logique / physique) : 512 octets / 512 octets

taille d'E/S (minimale / optimale) : 512 octets / 512 octets


[...]

Partitionner le disque sdb avec la commande fdisk /dev/sdb. Cela ouvre l'utilitaire de partitionnement.
En choisissant la commande m dans l'utilitaire, l'aide va apparaître :

Commande (m pour l'aide) : m

Aide :

  DOS (secteur d'amorçage)
   a   modifier un indicateur d'amorçage
   b   éditer l'étiquette BSD imbriquée du disque
   c   modifier l'indicateur de compatibilité DOS

  Générique
   d   supprimer la partition
   F   afficher l’espace libre non partitionné
   l   afficher les types de partitions connues
   n   ajouter une nouvelle partition
   p   afficher la table de partitions
   t   modifier le type d'une partition
   v   vérifier la table de partitions
   i   Afficher des renseignements sur la partition

  Autre
   m   afficher ce menu
   u   modifier les unités d'affichage et de saisie
   x   fonctions avancées (réservées aux spécialistes)

  Script
   I   chargement de l’agencement à partir du fichier de script sfdisk
   O   sauvegarde de l’agencement vers le fichier de script sfdisk

  Sauvegarder et quitter
   w   écrire la table sur le disque et quitter
   q   quitter sans enregistrer les modifications

  Créer une nouvelle étiquette
   g   créer une nouvelle table vide de partitions GPT
   G   créer une nouvelle table vide de partitions SGI (IRIX)
   o   créer une nouvelle table vide de partitions DOS
   s   créer une nouvelle table vide de partitions Sun

En te guidant avec cet utilitaire, ajoute une nouvelle partition (avec n) et créer une partition primaire qui prend la totalité du disque. Ensuite, modifie le type de la partition en RAID Linux
auto.
Quand tu as fait toutes ces actions, sauvegarde et quitte l'utilitaire.
Vérifie avec la commande lsblk que le disque est bien partitionné.
Si c'est le cas, tu as l'information comme ceci :

Périphérique Amorçage Début      Fin Secteurs Taille Id Type

/dev/sdb1              2048 10485759 10483712     5G fd RAID Linux autodétecté

Fais la même chose avec le 2ème disque /dev/sdc.

Avec lsblk tu dois avoir ceci :

wilder@Ubuntu:~$ lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS


[...]

                            /

sdb      8:16   0     5G  0 disk 

└─sdb1   8:17   0     5G  0 part 

sdc      8:32   0     5G  0 disk 

└─sdc1   8:33   0     5G  0 part 

sdd      8:48   0     5G  0 disk 

    Tu vois bien chaque partition sdb1 et sdc1.

Création du RAID

Exécute la commande sudo mdadm --create /dev/md0 --level 1 --raid-devices 2 /dev/sdb1 /dev/sdc1

Explications de cette commande :

    /dev/md0 : nom du périphérique RAID à créer
    --level 1 : niveau de RAID, soit ici du RAID 1 (on peut aussi écrire --level=1)
    --raid-devices 2 : le nombre de disques, soit ici 2 (on peut aussi écrire --raid-devices=2)
    /dev/sdb1 /dev/sdc1 : les partitions des disques concernés

Voici le résultat :

wilder@Ubuntu:~$ sudo mdadm --create /dev/md0 --level 1 --raid-devices 2 /dev/sdb1 /dev/sdc1

mdadm: Note: this array has metadata at the start and

    may not be suitable as a boot device.  If you plan to

    store '/boot' on this device please ensure that

    your boot-loader understands md/v1.x metadata, or use

    --metadata=0.90

Continue creating array? y

mdadm: Defaulting to version 1.2 metadata

mdadm: array /dev/md0 started.

Vérification

Pour vérifier l'état du RAID, il faut aller voir le contenu du fichier mdstat :

wilder@Ubuntu:~$ cat /proc/mdstat

Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10] 

md0 : active raid1 sdb1[0] sdc1[1]

      5236736 blocks super 1.2 [2/2] [UU]


      

unused devices: <none>

Le résultat de la commande donne un RAID 1 actif md0 avec les partitions sdb1 et sdc1.
De plus[UU] indique que les 2 disques sont en marche (Up).

Aller plus loin avec mdstat

Tous les détails sur ce fichier.
https://raid.wiki.kernel.org/index.php/Mdstat

Tu peux aussi voir l'état du RAID avec la commande mdadm :

wilder@Ubuntu:~$ sudo mdadm --detail /dev/md0

/dev/md0:

           Version : 1.2

     Creation Time : Wed Feb 08 10:41:03 2023

        Raid Level : raid1

        Array Size : 5236736 (4.99 GiB 5.36 GB)

     Used Dev Size : 5236736 (4.99 GiB 5.36 GB)

      Raid Devices : 2

     Total Devices : 2

       Persistence : Superblock is persistent


       Update Time : Wed Feb 08 10:41:30 2023

             State : clean 

    Active Devices : 2

   Working Devices : 2

    Failed Devices : 0

     Spare Devices : 0


Consistency Policy : resync


              Name : Ubuntu:0  (local to host Ubuntu)

              UUID : 79dcf1ad:b0244cd7:cf2aced4:f0e99d3b

            Events : 17


    Number   Major   Minor   RaidDevice State

       0       8       17        0      active sync   /dev/sdb1

       1       8       33        1      active sync   /dev/sdc1

    Ici le statut du RAID est clean et les partitions concernées sont bien /dev/sdb1 et /dev/sdc1.
    De plus, /dev/sdb1 et /dev/sdc1 sont bien synchronisés.

On peut aussi voir les disques inclus dans le RAID md0 avec la commande lsblk -f.

wilder@Ubuntu:~$ lsblk -f

NAME    FSTYPE            FSVER LABEL          UUID                                 FSAVAIL FSUSE% MOUNTPOINTS


[...]

                                                                                                   /

sdb                                                                                                

└─sdb1  linux_raid_member 1.2   Ubuntu:0 79dcf1ad-b024-4cd7-cf2a-ced4f0e99d3b                

  └─md0                                                                                            

sdc                                                                                                

└─sdc1  linux_raid_member 1.2   Ubuntu:0 79dcf1ad-b024-4cd7-cf2a-ced4f0e99d3b                

  └─md0                                                                                            

sdd

Formatage du RAID

Formates le volume RAID md0 avec un file system en ext4 et avec le nom PersonalData.

wilder@Ubuntu:~$ sudo mkfs.ext4 /dev/md0 -L "PersonalData"

mke2fs 1.46.5 (30-Dec-2021)

En train de créer un système de fichiers avec 1309184 4k blocs et 327680 i-noeuds.

UUID de système de fichiers=6446c9f7-5d25-43f4-bc94-18a09c7dd9b8

Superblocs de secours stockés sur les blocs : 

	32768, 98304, 163840, 229376, 294912, 819200, 884736


Allocation des tables de groupe : complété                            

Écriture des tables d'i-noeuds : complété                            

Création du journal (16384 blocs) : complété

Écriture des superblocs et de l'information de comptabilité du système de

fichiers : complété

Si tu relances la commande lsblk -f, tu vois que md0 est formaté en ext4 et que le nom est PersonalData :

wilder@Ubuntu:~$ lsblk -f

NAME   FSTYPE            FSVER LABEL    UUID                                 FSAVAIL FSUSE% MOUNTPOINTS

[...]

sdb                                                                                                

└─sdb1  linux_raid_member 1.2   Ubuntu:0 79dcf1ad-b024-4cd7-cf2a-ced4f0e99d3b                

  └─md0 ext4              1.0   PersonalData   6446c9f7-5d25-43f4-bc94-18a09c7dd9b8                

sdc                                                                                                

└─sdc1  linux_raid_member 1.2   Ubuntu:0 79dcf1ad-b024-4cd7-cf2a-ced4f0e99d3b                

  └─md0 ext4              1.0   PersonalData   6446c9f7-5d25-43f4-bc94-18a09c7dd9b8                

sdd   

[...]                                                                                                                           

Montage du RAID

Ajoute un dossier Data-Raid1 sous /home/wilder, pour monter la partition md0 :

wilder@Ubuntu:~$ sudo mkdir /home/wilder/Data-RAID1 -p

Montes ensuite la partition md0 dans ce dossier :

wilder@Ubuntu:~$ sudo mount /dev/md0 /home/wilder/Data-RAID1/

Pour que le RAID soit monté automatiquement à chaque démarrage, il faut ajouter la ligne suivante au fichier /etc/fstab :

/dev/md0 /home/wilder/Data-RAID1 ext4 nofail 0 0

Qu'indique cette ligne :

    /dev/md0 : Le RAID
    /home/wilder/Data-RAID1 : Le point de montage de la partition RAID
    ext4 : Le FS choisi pour le RAID
    nofail : Avec cette option, il n'y aura pas de blocage au boot s'il y a un problème avec md0.

En géneral, on met default, mais ici on met nofail pour éviter d'être bloqué au démarrage de la machine.

    0 : C'est un champ pour les options de sauvegarde. La valeur 0 signifie que le système de fichiers ne sera pas sauvegardé lors de l'exécution de la commande dump.
    0 : C'est un champ pour la fréquence de vérification du système de fichiers. La valeur 0 signifie que le système de fichiers ne sera pas vérifié lors du redémarrage.

Verrouillage du nom md0 de la partition RAID

La commande sudo mdadm --detail --scan donne le résultat suivant :

wilder@Ubuntu:~$ sudo mdadm --detail --scan

ARRAY /dev/md0 metadata=1.2 name=Ubuntu:0 UUID=79dcf1ad:b0244cd7:cf2aced4:f0e99d3b

Erreur de montage de RAID due à un mauvais nom de partition

Sans verrouillage du nom de la partition RAID, le système va changer le nom. Pour éviter cela, il faut utiliser l'UUID du périphérique. Sinon le nom md0 va se transformer en mdX avec X commençant à 127.
Un RAID qui n'est pas bien configuré et qui est mal monté au boot peut empêcher le système de démarrer.

Pour éviter la modification du nom de la partition RAID, ajouter la ligne ARRAY /dev/md0 metadata=1.2 name=Ubuntu:0 UUID=79dcf1ad:b0244cd7:cf2aced4:f0e99d3b au fichier /etc/mdadm/mdadm.conf
pour lier le nom md0 à son UUID.

En faisant cela, /etc/fstab va vérifier dans /etc/mdadm/mdadm.conf le lien entre le nom et l'UUID.
On peut aussi mettre directement l'UUID du périphérique dans /etc/fstab sous la forme :
UUID=79dcf1ad:b0244cd7:cf2aced4:f0e99d3b /media/wilder/Data-RAID1 ext4 nofail 0 0

Les UUIDs

Comment bien les utiliser dans fstab.
https://doc.ubuntu-fr.org/mount_fstab#note_sur_les_uuids

Forcer la mise en application des changements avec la commande sudo update-initramfs -u et redémarrer la machine.

Documentation sur la commande update-initramfs

La documentation pour Ubuntu 22.04 et pour les autres versions.
https://manpages.ubuntu.com/manpages/jammy/en/man8/update-initramfs.8.html

🔧 Test du bon fonctionnement du RAID 1

Créer un fichier ou un dossier sur la partition md0 qui est accessible directement depuis le home.

Les droits d'accès à ce nouveau système de fichier doivent probablement être paramétrés correctement en fonction des utilisateurs qui doivent pouvoir y accéder. Les commandes chmod et chown peuvent t'y aider.

🔧 Simulation d'une panne

Test cette possibilité en retirant un des 2 disques.

    Sur VirtualBox cette manipulation peut-être faite via l'interface graphique (Option Stockage dans la Configuration de la VM), à condition d'éteindre la machine. En effet, l'interface graphique permet de choisir la configuration au démarrage de la VM.

    Il est aussi possible de supprimer un disque en ligne (c'est à dire sans éteindre la VM) avec la ligne de commande VirtualBox.

    La commande vboxmanage showvminfo permet d'obtenir toutes les informations concernant une VM.
    Les informations concernant le Storage Controller utilisé ainsi que le port et l'identifiant UUID de chaque disque virtuel sont nécessaires pour la commande suivante.

wilder@ubuntu:~$ vboxmanage showvminfo "Ubuntu"

Name:                        Ubuntu

Groups:                      /

Guest OS:                    Ubuntu (64-bit)


[...]

Storage Controllers:

#0: 'IDE', Type: PIIX4, Instance: 0, Ports: 2 (max 2), Bootable

  Port 1, Unit 0: Empty

#1: 'SATA', Type: IntelAhci, Instance: 0, Ports: 4 (max 30), Bootable

  Port 0, Unit 0: UUID: 8cafd925-d47b-4c95-9d19-17bdf923563c

    Location: "/home/wilder/VirtualBox VMs/Ubuntu/Ubuntu.vmdk"

  Port 1, Unit 0: UUID: 253d36da-efb1-4394-9727-9433aeffad63, hot-pluggable

    Location: "/home/wilder/VirtualBox VMs/Ubuntu/Ubuntu_1.vmdk"

  Port 2, Unit 0: UUID: 4c19f253-d875-445e-bc8d-b50b38016269, hot-pluggable

    Location: "/home/wilder/VirtualBox VMs/Ubuntu/Ubuntu_2.vmdk"

  Port 3, Unit 0: UUID: b412c57c-fd21-4575-b819-874143a4c9a9, hot-pluggable

    Location: "/home/wilder/VirtualBox VMs/Ubuntu/Ubuntu_3.vmdk"

[...]

    On voit ainsi que cette VM, nommée Ubuntu, dispose de 2 contrôleurs de disques

        Un contrôleur 0 nommé IDE
        Un contrôleur 1 nommé SATA

    Le contrôleur SATA a 4 ports sur lesquels on trouve un disque :

        Un disque d'UUID 8cafd925-d47b-4c95-9d19-17bdf923563c sur le port 0
        Un disque d'UUID 253d36da-efb1-4394-9727-9433aeffad63 sur le port 1
        Un disque d'UUID 4c19f253-d875-445e-bc8d-b50b38016269 sur le port 2
        Un disque d'UUID b412c57c-fd21-4575-b819-874143a4c9a9 sur le port 3

    Chaque port du contrôleur n'est connecté qu'à 1 disque, mais il pourrait y en avoir plusieurs sur certains contrôleurs, ce qui explique le 0 dans Port X, Unit 0.

    Muni de ces informations, il est possible de connecter ou déconnecter des disques avec la commande vboxmanage storageattach

    Par exemple, pour déconnecter le dernier des 4 disques précédents, sur le port 3 donc, on indique à VirtualBox qu'on souhaite le remplacer par un medium : none avec la commande suivante :


wilder@ubuntu:~$ vboxmanage storageattach "Ubuntu" --storagectl SATA --port 3 --medium none

    Pour reconnecter le disque, on utilise la même commande, toujours en précisant la VM, le contrôleur disque et le port, mais en précisant en plus le type hdd et le UUID du disque en medium.


wilder@ubuntu:~$ VBoxManage storageattach "Ubuntu" --storagectl SATA --port 3 --type hdd --medium b412c57c-fd21-4575-b819-874143a4c9a9

La documentation VirtualBox

Plus de détail sur l'utilisation de VirtualBox en ligne de commande sur la doc officielle.
https://www.virtualbox.org/manual/ch08.html#vboxmanage-storageattach

Pour enlever un disque, tu peux aussi utiliser la commande sudo umount /dev/sdX directement dans la VM Ubuntu.

Vérification de la tolérance aux pannes

Une fois le disque retiré, vérifie que ton fichier est toujours présent, que tu peux y accéder et même le modifier ou ajouter d'autres fichiers.

On voit dans le résultat de la commande mdadm --detail /dev/md0 que la situation n'est pas normale car l'état du RAID est Degraded et il y a un disque manquant.

En effet, la perte du second disque entraînerait cette fois-ci la perte des données.
Reconstruction du RAID

On va utiliser le 3ème disque prévu pour le RAID, /dev/sdd qui a été mis de coté pour l'instant.
Comme pour les 2 autres disques sdb et sdc, créer une partition principal de la taille du disque, et de type RAID Linux auto pour pouvoir l'ajouter au RAID.

Pour ajouter ce disque au RAID existant md0, exécute la commande suivante :

wilder@Ubuntu:~$ sudo mdadm --manage /dev/md0 --add /dev/sdd1

mdadm: added /dev/sdd1

wilder@Ubuntu:~$

Tu peux vérifier avec la commande sudo mdadm --detail /dev/md0 que le RAID est reconstruit.

À noter que :

    Un moment de reconstruction est nécessaire à la recopie des informations sur l'autre disque. Ce temps peut être très long dans le cas d'un disque concernant beaucoup de données.

☝️ Résumé

Tu sais maintenant configurer des volumes RAID sur une distribution Linux Ubuntu.
Cette vidéo explique pas-à-pas un exercice presque identique.

💪 Challenge

Dans un premier temps, exécute les différentes actions de cette quête :

    Créer un RAID 1 avec 2 disques
    Reconstruire le RAID après une simulation de panne
    Une fois que tout est fonctionnel, refait-là, mais cette fois-ci avec un autre type de RAID, comme le RAID 5 par exemple.

🧐 Critères d'acceptation

    Création d'un RAID 1 fonctionnel
    Création d'un autre type de RAID fonctionnel
    Historique de la commande history (filtrées des commandes inutiles et des erreurs...) mis en solution.

