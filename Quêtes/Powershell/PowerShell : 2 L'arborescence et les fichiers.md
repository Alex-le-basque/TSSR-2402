Introduction

Naviguer dans l'arborescence de fichiers en PowerShell et manipuler les fichiers est beaucoup plus rapide en ligne de commande qu'en mode graphique.
Nous allons voir ça ensemble !

image
🤓 Objectifs :

    ✅ Comprendre comment afficher le contenu d'un répertoire
    ✅ Savoir se repérer dans l'arborescence
    ✅ Comprendre comment naviguer dans l'arborescence
    ✅ Connaître ce que sont les arguments ou les paramètres d'une commande

💻 Quel OS je peux utiliser ?

Quel que soit l'OS (Windows, Linux, ou bien Mac OS), tu peux exécuter les commandes ci-dessous.

Suivant l'OS que tu utiliseras, les fichiers ou dossiers affichés ne seront pas les même.

😌 Quelle console prendre ?

    L'éditeur natif PowerShell ISE
    Le terminal PowerShell
    Un éditeur externe comme Visual Studio Code par exemple

Ce qui va suivre va être exécuté sous l'OS Windows. Tu dois adapter les emplacements si tu exécute PowerShell sous un autre OS. De plus, les dossiers vu seront alors différents.

💻 Lancement de l'éditeur PowerShell ISE

    Chercher Windows Powershell ISE
    La console se lance
    La partie bleue (en bas) est le terminal, la partie blanche (en haut) est la console de script
    Aller dans le menu Afficher puis cliquer sur Afficher le volet de script pour afficher/masquer la partie blanche en haut

⌨️​ Premières commandes

    Dans le terminal, taper Get-Location.

Get-Location

Ce qui donne en résultat:

Path          
----          
C:\Users\wilder

Get-Location donne l'emplacement courant. Soit ici c:\Users\wilder (si le nom que tu utilises pour te connecter à ton ordinateur est wilder)

    Maintenant, ecrire Get-Host

Get-Host

On obtient :

Name             : Windows PowerShell ISE Host
Version          : 5.1.22000.653
InstanceId       : 7736a470-1f2b-4877-a5e4-9ded82c7ed00
UI               : System.Management.Automation.Internal.Host.InternalHostUserInterface
CurrentCulture   : fr-FR
CurrentUICulture : fr-FR
PrivateData      : Microsoft.PowerShell.Host.ISE.ISEOptions
DebuggerEnabled  : True
IsRunspacePushed : False
Runspace         : System.Management.Automation.Runspaces.LocalRunspace

Soit des indication sur l'hôte:

    La version de PowerShell
    La langue, ...

📁​ Changer le répertoire courant

    Maintenant, changeons l'emplacement courant à la racine : C:\

On utilise le cmdlet Set-Location

Set-Location -Path C:\

Il ne se passe rien, mais le prompt indique PS C:\> au lieu de PS C:\Users\wilder\>.
Pour vérifier, on peut réutiliser Get-Location :

Get-Location

Résultat:

Path
----
C:\ 

Le répertoire courant a bien changé.

Note : il est possible de rappeler une commande précédente avec la touche flèche vers le haut ⬆️.

📁​ Créer des dossiers

Utilisation du cmdlet New-Item :

New-Item -Path Temp -ItemType Directory

On demande:

    Dans le répertoire courant, la création d'un répertoire
    son nom est Temp

Répertoire : C:\


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     18:03                Temp  

Plaçons nous dans ce dossier :

Set-Location Temp

Créons 2 dossiers Rep1 et Rep2:

New-Item -Path Rep1 -ItemType Directory

    Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:07                Rep1 

New-Item -Path Rep2 -ItemType Directory

    Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:08                Rep2 

⌨️​ Lister le contenu d'un dossier

Aller dans le dossier C:\Temp si besoin

Set-Location C:\Temp

Maintenant, utilisons la commande Get-ChildItem

Get-ChildItem

Résultat:

    Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
    Répertoire : C:\Temp
d-----        12/05/2022     12:07                Rep1                                                                                                              
d-----        12/05/2022     12:12                Rep2  

📄​ Créer des fichiers

Nous utiliserons encore le cmdlet New-Item mais cette fois-ci avec la valeur File pour l'attribut ItemType :

New-Item -Path file1 -ItemType File

Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
-a----        12/05/2022     12:15              0 file1  

Créons 2 autres fichiers file2 et file3:

New-Item -Path file2 -ItemType File

New-Item -Path file3 -ItemType File

Regardons le contenu du répertoire courant:

Get-ChildItem

Résultat:

    Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:07                Rep1                                                                                                              
d-----        12/05/2022     12:12                Rep2                                                                                                              
-a----        12/05/2022     12:15              0 file1                                                                                                             
-a----        12/05/2022     12:20              0 file2                                                                                                             
-a----        12/05/2022     12:20              0 file3 

​🧺​ Déplacer des fichiers ou des dossiers

Nous utiliserons le cmdlet Move-Item.
Déplaçons le fichier file1 dans le dossier Rep1.

Move-Item -Path file1 -Destination Rep1

Il ne se passe rien à l'écran. Avec Get-ChildItem on va vérifier que le fichier file1 a bien été déplacé.

Get-ChildItem

Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:25                Rep1                                                                                                              
d-----        12/05/2022     12:12                Rep2                                                                                                              
-a----        12/05/2022     12:20              0 file2                                                                                                             
-a----        12/05/2022     12:20              0 file3

Le fichier file1 n'est plus là.
Allons voir dans le dossier Rep1 :

Get-ChildItem -Path Rep1

    Répertoire : C:\Temp\Rep1


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
-a----        12/05/2022     12:15              0 file1 

Utilisons l'attribut recurse pour voir tout le contenu d'un dossier, même des sous dossiers

Get-ChildItem -Recurse

Ce qui donne :

    Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:25                Rep1                                                                                                              
d-----        12/05/2022     12:12                Rep2                                                                                                              
-a----        12/05/2022     12:20              0 file2                                                                                                             
-a----        12/05/2022     12:20              0 file3                                                                                                             


    Répertoire : C:\Temp\Rep1


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
-a----        12/05/2022     12:15              0 file1

Maintenant, déplaçons le dossier Rep1 dans le dossier Rep2 :

Move-Item -Path Rep1 -Destination Rep2

Avec un Get-ChildItem associé à recurse sur le dossier C:\Temp :

 Get-ChildItem -recurse

On obtient:

    Répertoire : C:\Temp


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:37                Rep2                                                                                                              
-a----        12/05/2022     12:20              0 file2                                                                                                             
-a----        12/05/2022     12:20              0 file3                                                                                                             


    Répertoire : C:\Temp\Rep2


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
d-----        12/05/2022     12:25                Rep1                                                                                                              


    Répertoire : C:\Temp\Rep2\Rep1


Mode                 LastWriteTime         Length Name                                                                                                              
----                 -------------         ------ ----                                                                                                              
-a----        12/05/2022     12:15              0 file1                                                                                                             

🗑️ Suppression de fichiers et dossiers

On va utiliser Remove-Item.
Supprimons le fichier file3 :

Remove-Item -Path file3

Le fichier est supprimé et il ne se passe rien à l'écran.

Le procédé est le même pour la suppression de dossier vide.
Si le dossier n'est pas vide, il faut utiliser l'attribut recurse :

Remove-Item Rep2 -Recurse

Attention

Il n'y a pas de confirmation ni de mise à la corbeille des fichiers/dossiers supprimés.
Il n'est pas possible de récupérer un fichier ou un dossier supprimé.

📝 Quiz

# 1 - Quel cmdlet permet de changer l'emplacement du répertoire courant ?

Set-Directory

Set-Location

Change-Location

Get-Location

# 2 - Le cmdlet Get-Location

# 3 - Que va faire la commande suivante: New-Item -Path Rep4 -ItemType Directory ?

# 4 - Le cmdlet Get-Host

💪 Challenge : Manipuler les fichiers et dossiers

    Copier le code ci-dessous dans la fenêtre script de PowerShell ISE et l'executer (F5) :

Set-Location -Path C:\

#Une écriture possible pour la création d'un dossier

New-Item -ItemType Directory -Path C:\ -Name FolderTest1

#Une autre écriture possible pour la création d'un dossier

New-Item -ItemType Directory -Path C:\FolderTest2

#Création de fichier vide dans le dossier c:\FolderTest

New-Item -ItemType File -Path C:\FolderTest1 -Name File1

New-Item -ItemType File -Path C:\FolderTest1 -Name File2

New-Item -ItemType File -Path C:\FolderTest1 -Name File3

New-Item -ItemType File -Path C:\FolderTest1 -Name File4

New-Item -ItemType File -Path C:\FolderTest1 -Name File5

#Création de fichier vide dans le dossier c:\FolderTest2

$Count = 6

Do

{

    New-Item -ItemType File -Path C:\FolderTest2 -Name "File$Count"

    $Count++

}

While ($Count -lt 11)

    Ce code va créer des dossiers et des fichiers:
        Les fichiers File1 à File5 sont dans le dossier C:\FolderTest1
        Les fichiers File6 à File10 sont dans le dossier C:\FolderTest2

    Ta tâche est la suivante:
        Mettre les fichiers avec un numéro pair dans un dossier c:\EvenFolder
        Mettre les fichiers avec un numéro impair dans un dossier c:\OddFolder

🤓 Pour rappel:

    Copy-Item copie un fichier ou un dossier d'un emplacement à un autre
    Move-Item déplace un fichier ou un dossier d'un emplacement à un autre
    Remove-Item Supprime un fichier ou un dossier
    New-Item crée un dossier ou un fichier
    Get-ChildItem liste le contenu d'un dossier

Une fois la tâche réalisée pour partager ton résultat :

    Récupère l'historique des commandes que tu as tapées avec Get-History > historique.txt
    Récupère le contenu des dossiers avec Get-ChildItem -Path *Folder* -Recurse > listing.txt
    Partage ces fichiers via une URL, par exemple via un gist, un dépôt github

🧐 Critères d'acceptation

Il devra y avoir 2 fichiers avec les informations suivantes:

    Un fichier avec les différentes lignes de commandes
    Un fichier avec le contenu de chacun des 2 dossiers EvenFolder et OddFolder, les dossiers FolderTest1 et FolderTest2 n'apparaissant pas parce qu'ils sont vides.
