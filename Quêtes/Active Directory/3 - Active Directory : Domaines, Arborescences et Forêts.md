Introduction

Prenons un peu notre envol et intéressons-nous à la structure globale d’une architecture Active Directory, où l’on trouvera potentiellement plusieurs domaines, des arbres et une forêt.

Nous avons déjà évoqué la notion de domaine, mais voyons ce qu’est un arbre et ce qu’est une forêt, et la position d’un domaine dans tout ça.

🤓 Objectifs :

✅ Comprendre le principe d'un Domaine
✅ Comprendre le principe d'une Forêt
👀 Contenu de la quête :

    La structure d’un Active Directory
    Représentation d'une Forêt AD

👉 La structure d'un Active Directory

AD offre trois niveaux principaux : les domaines, les arborescences et les forêts.

Un domaine est un groupe dans lequel sont reliés différents utilisateurs, ordinateurs et objets AD, comme les objets AD du siège social de ton entreprise.
Plusieurs domaines peuvent être combinés dans une arborescence, et un regroupement d’arborescences constitue une forêt.

N’oublie pas qu’un domaine représente un périmètre de gestion. Les objets pour un domaine donné sont stockés dans une base de données unique et peuvent être gérés ensemble.

Une structure d'un  Active Directory est donc composée de :

    La forêt : structure hiérarchique d'un ou plusieurs domaines indépendants (ensemble de tous les sous domaines Active Directory).
    L''arborescence : domaine de toutes les ramifications. Par exemple, dans l'arbre wilders.lan, data.wilders.lan et dev.wilders.lan sont des sous-domaines de wilders.lan
    Le domaine : constitue les feuilles de l'arborescence. dev.wilders.lan peut-être un domaine au même titre que wilders.lan

👉 Représentation d'une Forêt AD

Une forêt est un périmètre de sécurité. Les objets de différentes forêts ne peuvent pas interagir les uns avec les autres, à moins que les administrateurs de chaque forêt ne créent une relation de confiance entre elles.


Dans le schéma ci-desuss on retrouve wilders.lan qui va contenir les formations que propose la Wildcodeschool, puis on à le domaine wildcodeschool.lan qui va contenir tout les campus de la Wildcodeschool.

Cette structure complète contient donc :

    Deux domaines wilders.lan et wildcodeschool.lan
    Chacun des domaines contient une arborescence
    L'ensemble des domaines, sous-domaines et arborescences forme la forêt

☝️ Résumé

La structure Active Directory est composée d'objets hiérarchisés contenus dans des Unités Organisationnelles (UO). Il y a trois degrés composant l'arborescence :

    La forêt regroupe de façon hiérarchisée un ou plusieurs domaines indépendants, et donc l'ensemble des sous domaines compris dans l'Active Directory.
    L'arbre ou arborescence contient tous les sous-domaines dans des ramifications au sein du domaine principal.
    Le domaine, la plus petite unité, représente les feuilles de l'arbre. il peut s'agir de fichiers, par exemple.
