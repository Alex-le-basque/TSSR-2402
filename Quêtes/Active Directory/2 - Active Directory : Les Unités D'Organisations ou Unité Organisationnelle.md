L’unité d’organisation également appelée OU (Organizational Unit) est couramment utilisé dans le monde de l'entreprise.
Cependant, certains administrateurs utilisent les Organizational Unit comme des dossiers pour classer les objets d’Active Directory. Dans cette quête, nous allons donc revoir l'utilisation des "Organizational Unit".
🤓 Objectifs :

✅ Connaître le but d'une Unité d'Organisation
✅ Savoir utiliser une Unité d'Organisation au sein de l'Active Directory
👀 Contenu de la quête :

    But d'une Unité d'Organisation
    L'arborescence type d'une Unité d'Organisation pour des comptes utilisateurs
    L'arborescence type d'une Unité d'Organisation pour des comptes ordinateurs

👉 Le but d'une Unité d'Organisation

Le réel objectif d’une Unité d'Organisation (OU), ce pourquoi elles ont été créées, est de déléguer les tâches d’administration et les droits sur les objets d’Active Directory. Les Organization Unit sont des conteneurs administratifs. Elles contiennent des objets ayant les mêmes besoins administratifs.

Rien à voir avec un classement par services ou autre. Les groupes sont faits pour cela, pas les Organization Unit.
Les objets qui seront administrés de la même manière devront être placés dans la même Organization Unit indépendamment du service et des besoins d’affichage. On n’utilise pas les Organization Unit pour classer les utilisateurs par exemple.
👉 L'arborescence type d'une Unité d’Organisation pour des comptes utilisateurs

Il convient de séparer les comptes utilisateurs et ordinateurs.
Prenons un exemple, une personne des ressources humaines pourrait avoir le besoin de désactiver un compte utilisateur. Cependant elle n’aura certainement pas le besoin de désactiver un compte ordinateur.

Nous pouvons donc envisager une Organization Unit « employés » regroupant les comptes utilisateurs.

Les administrateurs sont également des employés, mais il n’est peut-être pas souhaitable que la fameuse personne des ressources humaines puisse désactiver un compte d’administration.

Nous pouvons donc envisager une seconde Organization Unit « administrateurs » regroupant les comptes d’administration.

Dans le cas d’une entreprise multi-site, il est possible qu’elle dispose d’un support sur chaque site.
Prenons en compte le fait que le support du site B n’aura besoin que de gérer les comptes du site B.

Dans ce cas des sous-Organization Unit à l’Organization Unit « employés » peuvent être envisagés comme l’Organization Unit « Montpellier » regroupant les utilisateurs du site de Montpellier et l’Organization Unit  « Nice » regroupant les
utilisateurs du site de Nice.

Voici ce à quoi pourrait ressembler notre arborescence d’Organization Unit pour la gestion des comptes utilisateurs :

image.png

Évidemment c’est à l'administrateur de décider de l’arborescence à adopter.
👉 L'arborescence type d'une Unité d’Organisation pour des comptes ordinateurs

Pour les comptes utilisateurs, il est conseillé de séparer utilisateurs standards et comptes d’administration. Nous allons procéder de manière similaire pour les comptes ordinateurs.
Ainsi, créer une Organization Unit « machines_clientes » regroupant tous les postes utilisateurs et une Organization Unit « serveurs » regroupant tous les serveurs pourrait être intéressant.

En effet, l’administration des serveurs n’est certainement pas gérée par les mêmes personnes que l’administration des postes clients.

À l’identique des comptes utilisateurs, nous pouvons redécouper l’Organization Unit « machines_clientes » en sous-Organization Unit représentant les différents sites, ceci dans le but de favoriser la gestion par un éventuel service informatique décentralisé.

Avec l’augmentation de la mobilité, les entreprises enregistrent de plus en plus d’ordinateurs portables. Il pourrait donc être intéressant de segmenter ces Organization Unit en fonction de la mobilité avec  « ordinateurs_portables » et « ordinateur_fixes ».

La gestion des serveurs quand à elle ne dépendra que très peu des sites de l’entreprise, mais plutôt du type de serveur. Il sera ainsi possible de segmenter l’Organization Unit « serveur » en sous-Organization Unit représentant les différents types de serveurs.

Voici ce que donnerait l’arborescence d’Organization Unit d’une telle gestion des comptes :

image.png
☝️ Résumé

Une unité organisationnelle permet de regrouper l’autorité dans un sous-ensemble de ressources d’un domaine. Une UO assure la fonction de barrière de sécurité pour les privilèges élevés et les autorisations. Les UO être utilisées pour mettre en place et limiter la sécurité et les rôles au sein des groupes
💪 Challenge

Sur ton Active Directory que tu as installé et configuré dans la quête précédente avec pour domaine wilders.lan

    Créer une Unité d'Organisation Wilders_students
    Créer un Groupe d'utilisateurs Students
    Créer un utilisateur au sein de ce groupe
    Documenter cette procédure et publie cette documentation en markdown en solution à cette quête

🧐 Critères d'acceptation

    La documentation proposée permet bien de créer une Unité d'Organisation Wilders.students, un groupe d'utilisateurs students et un utilisateur dans ce groupe.

