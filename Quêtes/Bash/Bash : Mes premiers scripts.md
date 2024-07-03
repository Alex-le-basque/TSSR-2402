Introduction

La possibilité offerte par les shells et Bash en particulier de créer des scripts pour automatiser des tâches en font des outils de choix pour les administrateurs systèmes.

Cette quête est un tremplin pour se lancer dans l'écriture de ses premiers scripts.

image
🤓 Objectifs :

✅ Comprendre les opérateurs logiques en programmation
✅ Savoir créer un script avec une interaction utilisateur
👀 Contenu de la quête :

    Un petit rappel
    Le scripting Bash
    Les variables
    Les opérateurs de comparaison
    Les conditions
    Les opérateurs logiques
    Les boucles
    Des exercices

👉 Un petit rappel
Qu'est-ce que Bash ?

Développé par la Free Software Foundation (FSF) dans le cadre du GNU Project, Bash est très réputé au sein de la communauté du logiciel libre. Il s’agit aujourd’hui du shell par défaut de la plupart des distributions GNU/Linux.

C’est donc un outil informatique qu’il est important de connaître et de maîtriser. C’est à la fois un interpréteur de commandes et un langage de programmation.

Bash est exécuté par différents terminaux, comme GNOME Terminal ou Console sur Linux et iTerm2 sur macOS.

Il est d'ailleurs possible d'avoir Bash aussi sous Windows, notamment via le Sous-système Windows pour Linux comme expliqué dans cet article

Dès lors que l’on démarre un terminal exécutant le shell bash, une invite de commande ou prompt apparaît. Il s’agit d’un symbole. En général, ce symbole est le signe dollar : $

Ce prompt indique que le shell attend l'entrée d'une commande.

Ainsi, bash est une application dont la fonction principale est d’exécuter d’autres applications installées sur un même système.

Pour apprendre à utiliser bash, il faut aussi apprendre les commandes les plus couramment installées et utilisées sur le système d’exploitation sur lequel il est lancé.
👉 Le scripting Bash

Un des facteurs de la popularité de bash est la possibilité d'écrire des scripts avancés. Tout ce qui peut être entré manuellement après le prompt peut aussi être listé dans un fichier texte pouvant être lancé (et relancé) ultérieurement.

Plutôt que de passer des heures à lancer manuellement des centaines de commandes, il est donc possible de scripter ces commandes et de laisser l’ordinateur les exécuter. Et puisque GNU/Linux est presque entièrement basé sur le shell bash, presque tout peut y être scripté.

Ceci offre de nombreuses possibilités d’automatisation. Les utilisateurs de GNU/Linux gagnent un temps fou grâce au scripting en automatisant leurs propres flux de travail.
👉 Les variables

Le nom d'une variable est un simple pointeur vers l'emplacement mémoire où sont conservées les données qu'elle contient.

Les variables qu'on crée dans un script (ou dans le terminal) sont localisées dans ce script (ou à l'ouverture d'un terminal), c'est-à-dire qu'elles ne sont utilisables que lorsqu'on exécute son script, (ou que l'on appelle la valeur d'une variable qu'on vient de déclarer dans un terminal).

Mais comment enregistrer une valeur en mémoire ?

Affectation directe :

#!/bin/bash


my_school="WildCodeSchool"

La valeur WildCodeSchool à été mémorisée dans my_school.

Pour “utiliser” une variable, c'est à dire pour en récupérer la valeur, on se sert de son nom.

Cela se fait avec le caractère spécial $ accolé au nom de la variable :

#!/bin/bash


echo $my_school # La sortie après avoir lancer ce script sera : WildCodeSchool

Les variables ne sont pas typées en bash, c'est à dire que :

#!/bin/bash


ma_variable=4

ma_variable="Strings"

ma_variable="Une string contenant des espaces"

ma_variable1=$ma_variable

La valeur d'une variable peut être un nombre, un ou plusieurs caractères, du texte espacé, une commande, la valeur d'une variable.

Il est également possible de stocker une valeur entrée par un utilisateur avec la méthode read

    En une ligne :

#!/bin/bash


read -p "Merci de spécifier votre âge : " age_user && echo "Vous avez $age_user ans "

    En deux lignes :

#!/bin/bash


read -p "Merci de spécifier votre âge : " age_user

echo "Vous avez $age_user ans"

    En trois lignes :

#!/bin/bash


echo "Merci de spécifier votre âge : "

read age_user

echo "Vous avez $age_user ans"

Les exemples ci-dessus illustre le fait qu'il existe toujours plusieurs façons d'écrire un script permettant d'avoir le même résultat.
👉 Les opérateurs de comparaison

Un opérateur de comparaison binaire compare deux valeurs (souvent stockées dans des variables). Notez que la comparaison entre entiers et chaînes de caractères utilisent un ensemble différent d'opérateurs.
Comparaison de d'entiers.
Opérateur	Comparaison
-eq	est égal à
-ne	n'est pas égal à
-gt	est plus grand que
-lt	est plus petit que
-ge	est plus grand ou égal à
-le	est plus petit ou égal à
<	est plus petit que (à l'intérieur de parenthèses doubles)
<=	est plus petit ou égal à (à l'intérieur de parenthèses doubles)
>	est plus grand que (à l'intérieur de parenthèses doubles)
>=	est plus grand ou égal à (à l'intérieur de parenthèses doubles)
Comparaison de chaînes de caractères
Opérateur	Comparaison
=	est égal à
==	est égal à
!=	n'est pas égal à
<	est plus petit que (d'après l'ordre alphabétique ASCII)
>	est plus grand que (d'après l'ordre alphabétique ASCII)
-n	La chaîne de caractères n'est pas vide
-z	La chaîne de caractères est vide, c'est à dire qu'elle à une taille nulle
👉 Les conditions

En Bash, il n'y a pas d’indentation ou d’accolades, un if se démarque par des mots clefs de début et de fin.

Le if teste uniquement le code de retour de la commande (0 étant le code de succès).
Il est néanmoins souvent accompagné d’une commande de test.

Cette commande est test, ou [

Note bien que contrairement aux langages de la famille C, les crochets [] utilisés pour les tests sont bien une commande et non une structure de langage.

if [ -f config.php ]

then

    echo 'action if config.php exists'

else

    echo 'action if config.php does not exist'

fi


# Est pareil que 


if test -f config.php

then

    echo 'action if config.php exists'

else

    echo 'action if config.php does not exist'

fi

Conditions sur les fichiers

Permet de vérifier si un fichier ou un répertoire existe, sa nature etc. Nous verrons les différentes valeurs de conditions dans la suite de la quête.

if [ -f mon_fichier ]

then

    echo "Il s'agit bien d'un fichier présent"

fi

Conditions sur les strings

Pour traiter des chaînes de caractères.

variable_vide=""

if [ -z $variable_vide ]

then

    echo "la variable est bien vide"

fi


if [ $variable = "bonjour" ]

then

    echo "la variable contient la chaîne bonjour"

fi

Conditions arithmétiques

Pour comparer des entiers dans une condition :

num=0

if [ $num -lt 1 ]

then

    echo "la variable est inférieure à 1"

fi


num=100

if [ $num -eq 100 ]

then

    echo "la variable est égale à 100"

fi

👉 Les opérateurs logiques

La plupart des langages de programmation permettent d'associer les conditions à l'aide d'opérateurs logiques.

Les opérateurs logiques permettent de combiner deux conditions pour en former une nouvelle, plus complexe.

Par exemple la condition "SI ceci ET cela" est formée de deux conditions ("ceci" et "cela") qui sont combinées à l'aide d'un opérateur logique qui est "ET".

Il est possible d'assembler 2 conditions entre-elles pour former une condition plus complexe qui n'est valide que si les deux sous-conditions sont vérifiées. C'est l'opérateur ET qui s'écrit &&

#!/bin/bash

if [ $prenom = "john" ] && [ $nom = "doe" ]

then

   echo "Bonjour John Doe"

fi

L'opérateur OU, qui s'écrit ||, est similaire à && mais est valide si au moins une des conditions est vérifiée.

#!/bin/bash

if [ $jour = "samedi" ] || [ $jour = "dimanche" ]

then

  echo "C'est le week-end"

fi

L'instruction sera exécutée si la variable jour a pour valeur "samedi" ou "dimanche".
👉 Les boucles
La boucle
for

La boucle for permet de parcourir une liste de valeurs, elle effectue donc un nombre de tours de boucle qui est connu à l'avance.

#!/bin/bash

liste_utilisateur=("Jean Paul Pierre")

for utilisateur in $liste_utilisateur

    do echo "Bonjour " $utilisateur

done

Output :

Bonjour Jean

Bonjour Paul

Bonjour Pierre

La boucle
while

La boucle while exécute un bloc d'instructions tant qu'une certaine condition est satisfaite, lorsque cette condition devient fausse la boucle se termine. Cette boucle permet donc de faire un nombre indéterminé de tours de boucle, voire infini si la condition ne devient jamais fausse.

Par exemple tant que $i est plus petit que 10

let i=0

while [ $i -lt 10 ]

do echo "La valeur de i =" $i

    let i=$i+1

done

Output :

La valeur de i = 0

La valeur de i = 1

La valeur de i = 2

La valeur de i = 3

La valeur de i = 4

La valeur de i = 5

La valeur de i = 6

La valeur de i = 7

La valeur de i = 8

La valeur de i = 9

🔬 Exercices
1. Création d'un script permettant de compter jusqu'à un nombre donné

Le script demande à l'utilisateur une limite puis affiche toutes les valeurs comprises entre 1 et cette limite (incluse)
2. Création d'un script de machine à café

    La machine propose de choisir parmi 3 boissons (café, thé, chocolat) ou de choisir x pour sortir
    La machine vérifie si mon choix est valide et m'indique ce qu'elle me sert ou me dit que mon choix est invalide puis propose de boire une autre boisson jusqu'à ce que l'utilisateur tape `x

☝️ Résumé

Les scripts Bash sont un outil particulièrement utile, notamment à l'administrateur système.
Ils permettent de raccourcir les tâches répétitives en une seule commande.

💪 Challenge
Création d'un script de sauvegarde

Le script demande quel dossier l'utilisateur souhaite sauvegarder

Si le dossier n'existe pas, il affiche un message d'erreur

Le script demande ensuite où sauvegarder le fichier

Le script demande confirmation de sauvegarder à l'endroit choisit.

Le cas échéant, le script créé le dossier.

Le script affiche un message quand la sauvegarde est correctement effectuée

Le script demande si l'utilisateur veux sauvegarder un autre dossier

Copier dans l'éditeur de code fourni le script, une fois conçu et testé sur son ordinateur.
🧐 Critères d'acceptation

Le script proposé répond correctement aux contraintes imposées dans le challenge.
