🤓 Objectifs :

✅ Savoir ce qu'est un Active Directory
✅ Connaître les principaux services de l'Active Directory
✅ Connaître le rôle du contrôleur de domaine
👀 Contenu de la quête :

    L'histoire de l'Active Directory
    Définition et Explication de l'AD DS
    Installation du rôle AD DS

👉 L'histoire de l'Active Directory

L'Active Directory fut présenté pour la première fois en 1996, mais sa première utilisation remonte à Windows 2000 Server Édition en 1999. Les administrateurs informatiques travaillent avec depuis l’introduction de cette technologie.
L'Active Directory est le résultat de l'évolution de la base de données de comptes de domaine SAM (Security Account Manager) et une mise en œuvre de LDAP, protocole de hiérarchie.
Sa technologie de stockage est fondée sur le stockage du registre Windows, la base SAM constituant à elle seule une ruche, ce qui physiquement correspond à un fichier portant le nom (SAM) tout comme les fichiers system et software.
👉 Définition et Explication de l'AD DS

Les AD DS (Active Directory Domain Services) constituent les fonctions essentielles d’Active Directory pour gérer les utilisateurs et les ordinateurs et pour permettre aux administrateurs système d’organiser les données en hiérarchies logiques.

AD DS fournit des certificats de sécurité, l’authentification unique (SSO), LDAP, et la gestion des droits.

Parenthèse cybersécurité

La compréhension d’AD DS est une priorité absolue pour les professionnels de la réponse aux incidents et de la cybersécurité. En effet, la plupart des cyberattaques affectent AD et lorsqu’elles se produisent, il faut savoir ce qu’il faut rechercher et comment y répondre.

Les avantages des services de domaine avec l'Active Directory

Pour l’administration de base de ses utilisateurs et ordinateurs réseau, l’utilisation d’AD DS présente plusieurs avantages.

    Tu peux personnaliser la façon dont tes données sont organisées de façon à répondre aux besoins de ton entreprise
    Si cela s’avère nécessaire, tu peux gérer AD DS à partir de n’importe quel ordinateur du réseau
    AD DS fournit une fonction intégrée de réplication et de redondance : si un contrôleur de domaine tombe en panne, un autre contrôleur de domaine prend la charge à son compte
    Tout accès aux ressources réseau passe par AD DS, ce qui assure une gestion centralisée des droits d’accès au réseau

Les termes à connaître

Pour comprendre l'Active Directory Domain Services, il faut définir quelques termes clés.

    Schéma : l’ensemble de règles utilisateur configurées qui régissent les objets et attributs dans AD DS.
    Catalogue Global : le conteneur de tous les objets AD DS. Si tu as besoin de trouver le nom d’un utilisateur, ce nom est stocké dans le catalogue global.
    Mécanisme de requêtes et d'index : ce système permet aux utilisateurs de se trouver les uns et les autres dans AD.
    Par exemple, lorsque tu commences à saisir un nom dans ton client de messagerie, ce dernier te propose les correspondances possibles.
    Services de réplication : le service de réplication garantit que tous les contrôleurs de domaine du réseau partagent le même catalogue global et le même schéma.
    Sites : les sites sont des représentations de la topologie réseau ; AD DS sait ainsi quels sont les objets qui vont ensemble, ce qui lui permet d’optimiser la réplication et l’indexation.
    Lightweight Directory Access Protocol (LDAP) : protocole qui permet à AD de communiquer avec d’autres services d’annuaire sur d’autres plates-formes.

Quels sont les services fournis par Active Directory Domain Services ?

Les services fournis par AD DS suivants constituent les fonctionnalités de base d’un système de gestion centralisée des utilisateurs.

    Services de domaines : Stocke les données et gère les communications entre les utilisateurs et le contrôleur de domaine. Il s’agit de la principale fonctionnalité d’AD DS.
    Services de certificat : Permet à ton contrôleur de domaine de servir des certificats et des signatures numériques, ainsi qu’un chiffrement à clé publique.
    Lightweight Directory Services : Prend en charge LDAP pour des services de domaine multiplateformes, par exemple l’ensemble des ordinateurs Linux présents sur ton réseau.
    Services de fédération d'annuaire : Dans la même session, fournit une authentification SSO pour plusieurs applications. Ainsi, les utilisateurs ne sont pas obligés de ressaisir les mêmes identifiants.
    Gestion des droits : Contrôle les politiques en matière de droits à l’information et d’accès aux données. Par exemple, la gestion des droits détermine si tu peux accéder à un dossier ou envoyer un e-mail.

👉 Rôle des contrôleurs de domaine avec AD DS
Est ce que Kerberos chiffre ses tickets ?

[] Oui
[x] Non

    C'est le KDC qui se charge de vérifier et chiffrer les tickets de Kerberos utilisés par AD DS pour l'authentification (Chapitre Rôle des contrôleurs de domaine)
    Les contrôleur de domaine (CD) ou en anglais DC pour Domain Controller, sont les serveurs de ton réseau qui hébergent AD DS.
    Les CD répondent aux demandes d'authentification et stockent les données AD DS, ils hébergent également les services suivants, complémentaires à AD DS :

    KDC (Kerberos Key Distribution Center) : Le KDC vérifie et chiffre les tickets Kerberos utilisés par AD DS pour l'authentification
    NetLogon : NetLogon est le service de communication des informations d'authentification
    Windows Time (W32Time) : Kerberos impose que les horloges de tous les ordinateurs soient synchronisées. Ce service assure la syncrhonisation des horloges des ordinateurs du domaine.
    IsmServ (Intersite Messaging) : Intersite Messagin permert aux CD de communiquer les uns avec les autres pour la réplication et le routage inter-sites.

L'Active Directory doit avoir au moins un contrôleur de domaine. Les CD sont les conteneurs des domaines. Chaque domaine fait partie d’une fôret Active Directory, qui peut comporter un ou plusieurs domaines organisés en Unités Organisationnelles.

L'Active Directory Domain Services gère les relations de confiance entre plusieurs domaines, ce qui te permet d’accorder les droits d’accès des utilisateurs d’un domaine à d’autres domaines de sa forêt.
☝️ Résumé

Le concept le plus important à comprendre et à retenir est qu’Active Directory Domain Service est un cadre de gestion du domaine, et que l’ordinateur employé par les utilisateurs pour accéder à AD est le contrôleur de domaine.

La cybersécurité moderne repose sur une connaissance approfondie d’Active Directory. Active Directory est au cœur des capacités des pirates à réaliser des infiltrations, des déplacements latéraux et des exfiltrations de données.  Mais, aussi discrets et astucieux soient-ils, ces pirates laissent des traces dans les logs d’AD lorsqu’ils se déplacent sur ton réseau.

📒 Documentation

Créer un domaine Active Directory sur Windows Server

Ce tuto aborde les différentes étapes pour mettre en place graphiquement un contrôleur de domaine sur un serveur Windows
https://www.windows8facile.fr/ws2016-creer-domaine-active-directory-dns-dhcp/

Installer et configurer Active Directory avec PowerShell

La mise en place d'un contrôleur de domaine est aussi possible directement avec PowerShell comme expliqué dans cet article
https://remiflandrois.fr/2019/01/28/install-config-ad-powershell/

💪Challenge

Installe et configure Active Directory server en reprenant la machine virtuelle que tu as utilisé pour faire les quêtes sur le DNS et DHCP .

    Installer le rôle AD DS sur Windows Serveur 2012 R2
    Créer un domaine wilders.lan
    Explique la procédure que tu as utilisée dans une documentation qui permet de reproduire ton installation et que tu postera comme solution à cette quête

🧐 Critères d'acceptation

    La procédure permet effectivement de mettre en place un contrôleur de domaine
    Les explications sont claires et adaptées à un administrateur·rice débutant·e

