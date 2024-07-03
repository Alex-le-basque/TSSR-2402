Introduction

Docker est un outil permettant la gestion d'applications sous la forme de conteneurs.

Cette quête propose de découvrir les notions d'image et de conteneur et ainsi de commencer à utiliser Docker pour déployer des applications.

The Docker blue banner representing a whale carrying containers on its back

🤓 Objectifs :

✅ Découvrir les concepts fondamentaux de Docker
✅ Récupérer des images
✅ Exécuter des conteneurs
Sommaire

    👉 Installer Docker
    👉 Gérer les images
    👉 Lancer un conteneur
        🔬 Lancement d'un conteneur interactif
        🔬 Lancement d'un conteneur serveur
    👉 Poursuivre son exploration
    ☝️ Résumé
        📝 Quiz
    💪 Challenge
    🧐 Critères d'acceptation

👉 Installer Docker

Docker est constitué d'un serveur dockerd et d'une interface utilisateur qui peut-être en ligne de commande : la commande docker, ou graphique : Docker Desktop.
L'installation classique de Docker consiste à installer le serveur et au moins un client sur la même machine.

En pratique, Docker Desktop est bien plus qu'une interface graphique pour interagir avec Docker. En effet, comme Docker repose fortement sur des fonctionnalités spécifiques du noyau Linux (les namespaces et les cgroups notamment), une couche de virtualisation supplémentaire est nécessaire, sur un système non Linux, pour disposer d'un noyau Linux et ainsi de Docker. Une installation de Docker Desktop consiste donc aussi à installer une machine virtuelle Linux pour héberger le dockerd.

Première étape pour commencer à pratiquer avec Docker : l'installation.

Même si Docker Desktop est maintenant aussi disponible sous Linux, l'installation habituelle consiste à installer Docker Engine qui installe le serveur et le client CLI directement sur l'OS.

    https://docs.docker.com/engine/
    Documentation officielle de Docker Engine
    👉 Suis la partie de la documentation dédiée à l'installation sur ta distribution GNU/Linux.

La façon la plus simple de vérifier si ton installation est fonctionnelle est sans doute d'afficher la version de docker.

wilder@host:~$ docker --version

Docker version 20.10.23, build 7155243

Qu'est-ce que Docker - IBM France

Cet article permet d'appréhender ce qu'est Docker et à quoi il sert.
https://www.ibm.com/fr-fr/cloud/learn/docker

👉 Gérer les images

Une application distribuée via Docker est appelée une image.

Une image peut-être vue comme un paquet contenant un programme et toutes les dépendances nécessaires à son exécution.
C'est parce qu'une image est ainsi auto-suffisante, et ne dépend au final que de la présence du Docker Engine, et de rien d'autre, pour être exécutée, que Docker peut-être considéré comme un moyen fiable et simple de déployer des applications sur n'importe quel environnement.

Il est évidemment possible de construire ses propres images, mais pour l'instant, il est plus simple de récupérer une image existante.

Les images peuvent être publiées dans des dépôts.

Durant l'installation de Docker, tu as peut-être été amené à créer un compte sur Docker Hub.
Ce site, géré par la société Docker Inc. qui est aussi l'éditeur de Docker, héberge un grand nombre de dépôts.
Tu peux d'ailleurs l'utiliser pour publier tes propres images dans ton propre dépôt public ou privé.

Par défaut, Docker récupère les images sur Docker Hub.

La commande pour récupérer une image est docker image pull (ou docker pull en version courte) suivi du nom d'une image, éventuellement suivi d'un identifiant de version séparé par :.
Lorsque la version n'est pas précisée, Docker récupère la dernière version : latest.

Tu as peut-être déjà récupéré l'image hello-world lors de l'installation. Tu peux le vérifier en affichant les images disponibles sur ta machine avec docker image ls (ou docker images en raccourci).

Exemple :

wilder@host:~$ docker image ls

REPOSITORY                                         TAG         IMAGE ID       CREATED         SIZE

docker/getting-started                             latest      3e4394f6b72f   6 weeks ago     47MB

debian                                             latest      c4905f2a4f97   9 months ago    124MB

hello-world                                        latest      feb5d9fea6a5   16 months ago   13.3kB

Si ce n'est pas fait, récupère cette image :

wilder@host:~$ docker image pull hello-world

Using default tag: latest

latest: Pulling from library/hello-world

2db29710123e: Pull complete 

Digest: sha256:aa0cc8055b82dc2509bed2e19b275c8f463506616377219d9642221ab53cf9fe

Status: Downloaded newer image for hello-world:latest

docker.io/library/hello-world:latest

Il ne serait d'ailleurs pas très grave d'essayer de récupérer une image déjà présente localement. Docker vérifie avant et indique que la récupération n'est pas nécessaire si l'image dans la version demandée est déjà présente.

wilder@host:~$ docker image pull hello-world:latest

latest: Pulling from library/hello-world

Digest: sha256:aa0cc8055b82dc2509bed2e19b275c8f463506616377219d9642221ab53cf9fe

Status: Image is up to date for hello-world:latest

docker.io/library/hello-world:latest

Il est possible de supprimer des images locales avec la commande docker image rm (ou docker rmi en raccourci).
👉 Lancer un conteneur

L'exécution d'une image consiste pour Docker à créer un conteneur.

Un conteneur est une forme d'isolation de processus sur un système qui se traduit par le fait que ce processus, et donc tout ses fils, sont isolés du reste du système qu'il voit donc différemment des autres processus.

Par exemple :

    Les processus du conteneur ne voit pas l'ensemble du système de fichier, mais seulement ce qui est disponible dans l'image.
    Le conteneur a en général un autre nom d'hôte (hostname) que le système hôte
    Les identifiants de processus sont différents dans le conteneur qui ne voit ainsi que les processus fils du processus racine du conteneur (qui sera le pid 1)
    etc.

Pour exécuter une image, et donc créer un conteneur, on utilise la commande docker run suivi du nom de l'image (et éventuellement de la version) qu'on veut exécuter.

Test en exécutant l'image hello-world. Tu devrais obtenir quelque chose de similaire à ce qui suit :

wilder@host:~$ docker run hello-world


Hello from Docker!

This message shows that your installation appears to be working correctly.


To generate this message, Docker took the following steps:

 1. The Docker client contacted the Docker daemon.

 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.

    (amd64)

 3. The Docker daemon created a new container from that image which runs the

    executable that produces the output you are currently reading.

 4. The Docker daemon streamed that output to the Docker client, which sent it

    to your terminal.


To try something more ambitious, you can run an Ubuntu container with:

 $ docker run -it ubuntu bash


Share images, automate workflows, and more with a free Docker ID:

 https://hub.docker.com/


For more examples and ideas, visit:

 https://docs.docker.com/get-started/

Docker tente en général de rattraper les oublis.
Aussi, si on tente d'exécuter une image qui n'est pas disponible, il va la télécharger automatiquement.
C'est ce qu'indique le texte affiche par ce hello-world qui suppose qu'on a directement entré docker run hello-world juste après l'installation de Docker. Ce que tu as peut-être fait en suivant la doc 😉.

De très nombreuses options sont disponibles pour docker run et permettent de définir la façon dont Docker doit exécuter le conteneur.

Quelques exemples :

    -d : permet d'executer le conteneur en tâche de fond.
    -e : permet de passer au conteneur des variables d'environnement.
    -h : permet de choisir le nom d'hôte du conteneur.
    -i : exécution interactive. Les entrées clavier sont transmise au conteneur.
    -m : limite la quantité de mémoire vive accessible par le conteneur.
    -p : créé un transfert de port entre un port de l'hôte et un port du conteneur.
    -t : associe une console (pseudo-tty) au conteneur.
    -v : associer le conteneur à un volume.
    etc.

🔬 Lancement d'un conteneur interactif

Décortiquons l'exemple suggéré par hello-world d'exécuter docker run -it ubuntu bash

Il s'agit d'exécuter l'image ubuntu. Même plus précisément ubuntu:latest puisqu'aucune version n'est précisée. Cette image est comme son nom l'indique un système ubuntu minimal.
Il est d'ailleurs possible d'en savoir plus en allant voir son descriptif sur Docker Hub.
Les options -it permettent d'interagir directement avec le conteneur via le terminal.
Le bash final indique quelle commande/programme on souhaite exécuter dans le conteneur. Ce paramètre est optionnel car chaque image contient la commande à exécuter par défaut quand ce n'est pas précisé.

Lance la commande, tu devrais obtenir un résultat similaire à ceci :

wilder@host:~$ docker run -it ubuntu bash

Unable to find image 'ubuntu:latest' locally

latest: Pulling from library/ubuntu

677076032cca: Pull complete 

Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f

Status: Downloaded newer image for ubuntu:latest

root@7037ef56444a:/#       

Il s'agit d'un shell, ce qui est logique puisqu'on a exécuté bash.
Mais l'utilisateur est maintenant root. Par défaut, les processus sont exécutés par le compte root dans le conteneur.
On note que le nom d'hôte à changé. Il est très probablement différent sur ta machine, il s'agit de l'identifiant du conteneur que Docker à générer automatiquement lorsqu'il l'a lancé.

On peut d'ailleurs le vérifier en ouvrant un autre terminal et en utilisant la commande docker container ps (ou docker ps en version courte) qui affiche l'ensemble des conteneurs actifs :

wilder@host:~$ docker container ps

CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES

7037ef56444a   ubuntu    "bash"    5 minutes ago   Up 5 minutes             brave_galileo

Attention à bien exécuter docker ps dans un autre terminal car dans le conteneur, tu n'as accès qu'à ce qui se trouve dans l'image qui ne contient, bien sûr, pas Docker.

Tu peux d'ailleurs constater l'isolation du conteneur en affichant tous les processus à l'aide de la commande ps -ef et constater que l'exécution dans le conteneur produit un résultat bien différent d'une exécution sur le système hôte.

Dans le conteneur, tu obtiens quelque chose de similaire à ceci :

root@7037ef56444a:/# ps -ef

UID          PID    PPID  C STIME TTY          TIME CMD

root           1       0  0 15:13 pts/0    00:00:00 bash

root          12       1  0 15:21 pts/0    00:00:00 ps -ef

On constate bien qu'il n'y a que notre bash, et les commandes qu'il a exécutées (processus fils), qui s'exécute dans le conteneur et que bash est d'ailleurs le processus de pid 1.

De la même façon, le système de fichier vu par le conteneur est différent de celui de l'hôte. Par exemple le /home du conteneur est vide.

Taper exit dans le conteneur termine bash et passe le conteneur dans l'état exited.
Il n'apparaît d'ailleurs plus dans la liste des conteneurs affichée par docker ps sauf si on y ajoute l'option -a.

La commande docker system prune permet un nettoyage de certaines ressources docker plus utilisées, notamment les conteneurs terminés.
🔬 Lancement d'un conteneur serveur

Tentons maintenant d'exécuter un conteneur serveur.

Pour l'exemple, on suppose de déploiement d'un serveur web : le classique httpd, le serveur web de la fondation Apache.

Une image docker officielle de ce serveur est disponible sur Docker Hub : Image Docker de httpd.

Lance directement le conteneur avec la commande docker run -dp 8000:80 httpd.
L'option d pour l'executer en tâche de fond, et l'option p 8000:80 pour indiquer de mettre en port 8000 de l'hôte en écoute et de transferer les paquets qui y arrivent sur le port 80 du conteneur.

wilder@host:~$ docker run -dp 8000:80 httpd

Unable to find image 'httpd:latest' locally

latest: Pulling from library/httpd

01b5b2efb836: Pull complete 

831122b282b9: Pull complete 

1a6abe5420b4: Pull complete 

36fa1415f90a: Pull complete 

0127b4d49ca0: Pull complete 

Digest: sha256:e63470b5cf761fe43810b49a1cc3117746d7d6bff36d80e2b0a5ad1c6f0325d5

Status: Downloaded newer image for httpd:latest

c3ea8a097e236934cd265981ff7f080264044d2a28654455b36e901a013f9f07

wilder@host:~$

La commande a déclenché automatiquement le téléchargement de l'image.
On note même qu'il y a eu des téléchargements.
C'est lié à un mécanisme de gestion des images par couches.
En effet, les images Docker sont constituées d'une succession de couches, chaque couche étant mutualisée entre toutes les images qui la partage.

Par exemple :
L'image httpd:2.4 qu'on vient de récupérer (latest au moment de la rédaction de cette quête) est construite à partir d'une autre image : debian:bullseye-slim. Sur un système où cette image est déjà présente, elle n'est pas téléchargée à nouveau. Elle est juste indiquée comme étant la première couche de l'image httpd:2.4.

Docker rend immédiatement la main, puisqu'on a lancé le conteneur en tâche de fond, mais il est actuellement en cours.

Il apparaît d'ailleurs dans docker ps

wilder@host:~$ docker ps

CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS                                   NAMES

c3ea8a097e23   httpd     "httpd-foreground"   14 minutes ago   Up 14 minutes   0.0.0.0:8000->80/tcp, :::8000->80/tcp   inspiring_brahmagupta

On peut aussi vérifier que le port 8000 est bien ouvert sur le système hôte.

wilder@host:~$ ss -tl

State                    Recv-Q                   Send-Q                                     Local Address:Port                                       Peer Address:Port                  Process                   

LISTEN                   0                        4096                                             0.0.0.0:8000                                            0.0.0.0:*                                               

LISTEN                   0                        4096                                                [::]:8000                                               [::]:*                                              

Et surtout plus intéréssant, on peut maintenant se connecter sur le serveur web en insérant http://localhost:8000 dans la barre d'adresse.

Screenshot of a web browser showing "It works"

Docker offre la possibilité d’accéder aux journaux du conteneur à l'aide de docker container logs (ou docker logs en plus court) suivi de l'identifiant du conteneur. Lesquels font, dans ce cas, apparaître les requêtes HTTP reçues.

wilder@host:~$ docker container logs c3ea8a097e23

AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message

AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message

[Tue Feb 07 13:41:34.517240 2023] [mpm_event:notice] [pid 1:tid 140370447027520] AH00489: Apache/2.4.55 (Unix) configured -- resuming normal operations

[Tue Feb 07 13:41:34.517295 2023] [core:notice] [pid 1:tid 140370447027520] AH00094: Command line: 'httpd -D FOREGROUND'

172.17.0.1 - - [07/Feb/2023:13:51:28 +0000] "GET / HTTP/1.1" 304 -

172.17.0.1 - - [07/Feb/2023:13:52:21 +0000] "GET / HTTP/1.1" 200 45

La commande docker stop suivi de l'identifiant du conteneur permet d’arrêter le conteneur, et donc de couper le serveur web.
👉 Poursuivre son exploration

Docker 101 tutorial

Un tutoriel proposé par Docker, assez orienté pour les dev, mais qui peut te permettre d'en apprendre un peu plus sur Docker.
https://www.docker.com/101-tutorial/

Le tutoriel précédent contient notamment cette longue vidéo qui permet de bien comprendre ce qu'est en conteneur en explorant, par l'exemple, les mécanismes qui permettent sa mise en place.

CLI cheat sheet

Ce petit aide-mémoire permet de retrouver un ensemble courant de commande docker.
https://docs.docker.com/get-started/docker_cheatsheet.pdf

☝️ Résumé

Docker permet de récupérer des images d'applications et de les exécuter dans des conteneurs logiciels.

💪 Challenge

Exécute l'image docker php puis lance là dans un conteneur interactif.
Dans l'interpréteur php alors disponible, exécute la fonction

php > phpinfo(INFO_GENERAL);

Copie le résultat de cette commande comme solution.

Tu peux sortir de l'interpréteur php (et du conteneur) en tapant exit.

BONUS :
Profite que tu as maintenant un interpréteur php à ta disposition pour apprendre les rudiments de ce langage 😛

🧐 Critères d'acceptation

L'affichage obtenu contient une trentaine de ligne qui commencent par PHP Version =>...
