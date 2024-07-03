Introduction

Passer à côté des GPO, c’est passer à côté d’une grosse fonctionnalité d’Active Directory pour windows server.
Les GPO pour Group Policy Object permettent de centraliser la gestion des configurations. Plus besoin de passer sur chaque poste pour appliquer un paramètre. Grâce aux GPO tu peux tout gérer depuis ton PC en quelques clics de souris. Un système de priorité et d’étendue te permettra d’appliquer les GPO très précisément aux utilisateurs et ordinateurs désirés.

🤓 Objectifs :

✅ Comprendre ce qu'est une GPO
✅ Savoir créer une GPO dans l'Active Directory
✅ Déployer une GPO
👀 Contenu de la quête :

    Présentation
    Types de GPO
    Exemple d'une GPO

👉 Présentation

Dans un environnement Active Directory, il est bien entendu possible de créer et déployer des stratégies de groupes pour cibler les postes clients et les utilisateurs. En complément, il faut savoir qu'il existe la possibilité d'appliquer des paramètres GPO directement en local sur un poste, notamment s'il est hors domaine.

Voyons ce qu'est une stratégie de groupe locale et ce qui la différencie d'une stratégie de groupe Active Directory.
👉 Types de GPO
La Stratégie de groupe locale

Depuis Windows XP, toutes les versions de Windows intègrent les paramètres de configuration avec une organisation similaire et accessible localement au travers d'une console MMC. Ces paramètres de configuration sont ceux que l'on retrouve lorsque l'on crée une stratégie de groupe sur un domaine, organisé de la même façon, et qui sont utilisables pour personnaliser la machine ou le serveur sur lequel on se trouve.

Ces paramètres lorsqu'ils sont gérés en local, restent stockés en local, et l'on appelle cela la "Stratégie de groupe locale" ou "Local Group Policy", en anglais. Avec ce mode de fonctionnement, il n'y a rien de centralisé : le paramétrage s'effectue machine par machine, à la main. Autrement dit, cela peut s'avérer très gourmand en temps.

Si on souhaite configurer la stratégie de groupe locale de son PC, c'est tout simple : il suffit d'ouvrir la console gpedit.msc, soit via une commande Exécuter (touche Windows + R) ou en recherchant directement dans la barre de recherche de Windows.

image.png

Tu as alors accès à la stratégie de l'ordinateur local, que tu peux modifier à souhait. Cela peut s'avérer utile sur une machine hors domaine, c'est-à-dire en groupe de travail ("WORKGROUP" par défaut).

image.png

C'est peut-être la première fois que tu vois cette interface. Lorsque nous allons créer des stratégies de groupe Active Directory, tu verras que la console est très proche de celle-ci.

La stratégie de groupe locale est une possibilité intéressante pour tester une nouvelle configuration afin de préconfigurer et sécuriser au mieux tes équipements. La gestion machine par machine étant difficilement envisageable, comment faire pour gérer de façon centralisée les paramètres de stratégie de groupe ? C'est là qu'interviennent les stratégies de groupe Active Directory.
Stratégie de groupe Active Directory

À l'aide d'une console unique, on peut gérer différentes stratégies de groupe (GPO) à appliquer sur ses machines et ses utilisateurs.

Garde à l'esprit que la stratégie de groupe locle peut s'avérer utile pour tester une configuration avant de la déployer sur un ensemble de machines de son parc. Ensuite, lorsque la configuration est testée et validée, tu pourras la déployer sur ton infrastructure grâce à une stratégie de groupe Active Directory.

La configuration sera ainsi appliquée sur tes machines, automatiquement, ce qui te feras gagner un temps précieux et offre une énorme flexibilité.
👉 Exemple d'une GPO

La GPO suivante est une stratégie de groupe de type utilisateur :

image.png

On peut détailler la stratégie comme ceci :

    Le nom de la GPO qui va s'appliquer à un objet (Utilisateurs ou Groupes, UO etc)
    Quelle configuration (ici utilisateur)
    Dans quelle rubrique
    On désactive la possibilité aux utilisateurs d'avoir accès à l'invite de commande.

Dans la forêt intranet.lan, on retrouve donc l'Unité d'Organisation nommée "Employee-Bank" qui possède une GPO nommée "Employee-GPO"

image.png

Et dans le domaine Active Directory dans la rubrique, Utilisateurs et Ordinateurs Active Directory on retrouve :

image.png

    Le Domaine
    L'Unité D'organisation
    Les utilisateurs ou Groupes d'utilisateurs

En conclusion c'est à ces utilisateurs ou groupes d'utilisateurs que s'appliqueront la GPO nommée "Employee-GPO".
🔬 Exercice
Mettre en œuvre la stratégie de comptes

Command Tips : WINDOWS + R

secpol.msc

Définir une stratégie de mot de passe comme suit :
Description	Value
Historique des mots de passe	5
Durée de vie maximale du mot de passe	45
Durée de vie minimale du mot de passe	3
Longueur minimale du mot de passe	16
Le mot de passe doit respecter des exigences de complexité	Oui

Définir une stratégie de verrouillage des comptes comme suit :
Description	Value
Durée de verrouillage du compte	30'
Seuil de verrouillage du compte	5
Réinitialiser le compteur de verrouillage après	29'
Paramétrer la stratégie de groupe locale

    Empêcher de modifier l’arrière-plan du bureau
    Empêcher l’utilisation de Windows Media Player
    Interdire l’installation à partir de supports amovibles (clé USB, CD-Rom, DVD)
    Empêcher le lancement de certaines applications : Skype par exemple
    Interdire l’accès à l’invite de commandes
    Interdire l’accès au menu Exécuter du menu Démarrer
    Interdire l’accès à la base de registre
    Empêcher l’accès à Windows update

☝️ Résumé

Les GPO vous offrent le contrôle de votre parc informatique. Chaque paramètre appliqué par une GPO n’est pas modifiable par l’utilisateur. La gestion centralisée étant prioritaire à la gestion locale. Une fois prises en main les GPO deviennent un outil indispensable de l’infrastructure Active Directory, y compris sur un parc composé de très peu de machines.

Command Tips : Pour forcer l'application des GPO, dans une invit de commande en tant qu'Administrateur

C:\>gpupdate /force

💪 Challenge

    Sur ton Active Directory, créé une GPO nommée Wilders
    Modifies la GPO pour que les utilisateurs du groupe Wilders_Students ne puisse pas avoir accès au panneau de configuration de Windows.
    Poste la démarche de configuration et les tests effectué en guise de solution

🧐 Critères d'acceptation

    La GPO s'applique uniquement aux utilisateurs du groupe Wilders_Students
    Elle empèche effectivement aux utilisateurs de ce groupes d'accéder au panneau de configuration
