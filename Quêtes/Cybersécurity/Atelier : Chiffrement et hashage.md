Atelier chiffrement et hashage

Cet atelier a pour but de te faire découvrir les principes de base du chiffrement et du hashage.
Sommaire

    🔬 Le contexte
    🤓 Objectifs
    📚 Prérequis
    Instructions
        Chiffrement symétrique
            Chiffrement
            Déchiffrement
        Chiffrement asymétrique
            Génération des clés
            Chiffrement
            Déchiffrement
        Hash
            Calcul d'un hash
            Vérification d'un hash
            Les mots de passe
            Bruteforce
            Sécurité des algorithmes de hash
    💪 Conclusion

🔬 Le contexte

Cet atelier nécessite une machine Linux. Tu peux utiliser une machine virtuelle ou un conteneur Docker.

Pour cet atelier, nous allons utiliser une machine virtuelle Ubuntu Server 22.04, mais la plupart des distributions Linux sont similaires.
🤓 Objectifs

    Comprendre les principes de base du chiffrement et du hashage
    Chiffrer et déchiffrer des données
    Hasher et comparer des données
    Comprendre les différences entre chiffrement et hashage
    Comprendre les différences entre chiffrement symétrique et asymétrique

📚 Prérequis

    Avoir une machine Linux (éventuellement une VM)
    Avoir installé GPG (GnuPG) sur sa machine
    Avoir installé OpenSSL sur sa machine

Instructions

Tout d'abord, ouvre ton terminal et crée un dossier de travail et place-toi dedans.

Par exemple :

mkdir ~/ateliers/ && cd ~/ateliers/

Une fois dans ce dossier, tu peux créer un fichier nommé plaintext.txt avec le contenu suivant :

echo "Hello World!" > plaintext.txt

Tu as maintenant un fichier plaintext.txt qui contient la chaîne de caractères Hello World!. Nous allons utiliser ce fichier pour illustrer les différents concepts de cet atelier.
Chiffrement symétrique

Le chiffrement symétrique est un type de chiffrement qui utilise la même clé pour chiffrer et déchiffrer des données.
C'est le type de chiffrement le plus simple à comprendre et à mettre en place.
Chiffrement

Pour illustrer le chiffrement symétrique, nous allons utiliser OpenSSL (vérifie que tu l'as bien installé sur ta machine).

Tu vas chiffrer le fichier créé précédemment avec OpenSSL. Pour cela, il faut utiliser la commande enc, puis saisir le mot de passe de chiffrement deux fois.

openssl enc -aes-256-cbc -pbkdf2 -in plaintext.txt -out encrypted.txt

enter AES-256-CBC encryption password:

Verifying - enter AES-256-CBC encryption password:

Cette commande va chiffrer le fichier plaintext.txt avec l'algorithme AES-256 en mode CBC et va stocker le résultat dans le fichier encrypted.txt.
Le paramètre -pbkdf2 permet d'utiliser une dérivation de clé basée sur le mot de passe.

Tu peux vérifier que le fichier encrypted.txt est bien chiffré en affichant son contenu :

cat encrypted.txt 

Salted__�����we�(}�

oI���b���U�

Tu peux également vérifier que le fichier encrypted.txt est bien chiffré en affichant son contenu avec la commande hexdump. Cela te permettra de voir les octets qui composent le fichier.

hexdump -C encrypted.txt 

00000000  53 61 6c 74 65 64 5f 5f  9a 9d aa 92 d5 77 65 f7  |Salted__.....we.|

00000010  28 7d bd 0a 6f 49 b2 85  cb 16 62 15 be f5 82 55  |(}..oI....b....U|

00000020

Comme tu peux le voir, le fichier est bien chiffré et ne contient plus la chaîne de caractères Hello World!.
Déchiffrement

Pour déchiffrer le fichier encrypted.txt, il faut utiliser la commande enc avec le paramètre -d (pour decrypt).
Il faut également préciser l'algorithme de chiffrement utilisé avec le paramètre -aes-256-cbc
et le paramètre -pbkdf2 pour utiliser la dérivation de clé basée sur le mot de passe comme précédemment pour le chiffrement.

openssl enc -d -aes-256-cbc -pbkdf2 -in encrypted.txt -out decrypted.txt

enter AES-256-CBC decryption password:

Cette commande va déchiffrer le fichier encrypted.txt avec l'algorithme AES-256 en mode CBC et va stocker le résultat dans le fichier decrypted.txt.

Tu peux vérifier que le fichier decrypted.txt est bien déchiffré en affichant son contenu :

cat decrypted.txt

Hello World!

La sécurité du chiffrement symétrique repose sur la sécurité de la clé de chiffrement. Si celle-ci est trop faible, il est possible de la casser et donc de déchiffrer les données.

Chiffrement asymétrique

Le chiffrement asymétrique est un type de chiffrement qui utilise deux clés : une clé publique et une clé privée.
La clé publique est utilisée pour chiffrer les données et la clé privée est utilisée pour déchiffrer les données.

Par exemple, si Alice veut envoyer un message à Bob, elle va utiliser la clé publique de Bob pour chiffrer le message.
Bob pourra ensuite utiliser sa clé privée pour déchiffrer le message. Bob et Alice s'échangent donc uniquement leur clé
publique pour communiquer et utilisent leurs clés privées respectives pour déchiffrer les messages.
Génération des clés

Tu vas à nouveau utiliser OpenSSL pour générer une paire de clés publique/privée.

Pour cela, il faut utiliser la commande genrsa avec le paramètre -out pour spécifier le nom du fichier de sortie et le paramètre -aes256 pour utiliser l'algorithme AES-256 pour chiffrer la clé privée.

openssl genrsa -out private.pem -aes256 4096

Enter PEM pass phrase:

Verifying - Enter PEM pass phrase:

Ici, la commande va générer une clé privée de 4096 bits et va la stocker dans le fichier private.pem.
Le mot de passe de chiffrement de la clé privée sera demandé deux fois. Il est nécessaire pour déchiffrer la clé privée.

Dans un contexte de production, ce fichier est très important et doit être conservé en lieu sûr. Si quelqu'un a accès à ce fichier, il peut déchiffrer toutes les données chiffrées avec la clé privée.

Maintenant que tu as généré la clé privée, tu vas générer la clé publique associée. Pour cela, il faut utiliser la commande rsa avec le paramètre -in pour spécifier le fichier contenant la clé privée et le paramètre -pubout pour générer la clé publique.

openssl rsa -in private.pem -pubout -out public.pem

Enter pass phrase for private.pem:

writing RSA key

Ici, la commande va générer la clé publique associée à la clé privée contenue dans le fichier private.pem et va la stocker dans le fichier public.pem.
Le mot de passe de chiffrement de la clé privée sera demandé.
Chiffrement

Tu vas maintenant chiffrer le fichier plaintext.txt avec la clé publique contenue dans le fichier public.pem. Pour cela, il faut utiliser la commande pkeyutl avec le paramètre -encrypt pour chiffrer les données, le paramètre -pubin pour spécifier que la clé publique est contenue dans le fichier public.pem et le paramètre -inkey pour spécifier le fichier contenant la clé publique.

openssl pkeyutl -encrypt -pubin -inkey public.pem -in plaintext.txt -out encrypted_asymmetric.txt

Cette commande va chiffrer le fichier plaintext.txt avec la clé publique contenue dans le fichier public.pem et va stocker le résultat dans le fichier encrypted_asymmetric.txt.

Tu peux vérifier que le fichier encrypted_asymmetric.txt est bien chiffré en affichant son contenu :

hexdump -C encrypted_asymmetric.txt

00000000  4f 1b a8 fe 3e 1a d6 17  a6 87 d6 94 5c 99 1f 2f  |O...>.......\../|

00000010  92 c3 6c 8d 1d 8f e6 b5  b1 bb 38 9f 33 69 ae 1a  |..l.......8.3i..|

00000020  7f 1b c2 e5 a6 16 20 57  8a 08 6f 68 03 4a c7 9a  |...... W..oh.J..|

00000030  e0 f6 bb 37 b4 37 51 08  2e 13 99 80 5d 52 cd 7a  |...7.7Q.....]R.z|

00000040  0c bb ca b3 8c 96 a5 da  d9 d7 68 80 60 a0 ba 2b  |..........h.`..+|

00000050  f2 66 77 41 51 a6 97 da  81 5e 21 d4 07 7b 4e b0  |.fwAQ....^!..{N.|

00000060  e4 56 53 90 e0 22 54 e8  65 bd 4f a8 b5 ba dc 37  |.VS.."T.e.O....7|

00000070  af 72 e8 c0 4a 1b e2 8d  b0 0a 8d 2d f2 e8 56 7c  |.r..J......-..V||

00000080  df 6e 80 f4 49 6d a6 06  ae 4f 17 d2 5c a3 69 ff  |.n..Im...O..\.i.|

00000090  53 c6 90 e6 6c 65 ef 9b  87 d1 aa 31 ba 0b 2b 36  |S...le.....1..+6|

000000a0  75 07 af c3 ad e3 c6 6f  55 df ac a9 4d ec d1 13  |u......oU...M...|

000000b0  e9 07 cb 95 76 9d 73 05  4a 79 3f ab 00 72 9f 20  |....v.s.Jy?..r. |

000000c0  73 d8 db c9 2b bf 1f 88  30 0a bf 4f e5 f4 a7 54  |s...+...0..O...T|

000000d0  68 06 e9 ee 98 9e 88 3c  a4 86 37 06 65 35 30 ec  |h......<..7.e50.|

000000e0  75 42 23 b7 54 a6 a6 0a  f8 88 50 75 79 d7 03 70  |uB#.T.....Puy..p|

000000f0  53 47 2a 5f 1b 48 3b 89  ad 30 5f 5b 64 dd f2 fe  |SG*_.H;..0_[d...|

00000100  58 1c d3 95 8d de d2 90  e3 33 2a 84 99 d8 bf 0b  |X........3*.....|

00000110  ae 07 49 de b7 96 c8 6a  1d c4 9e 69 07 ba ae 86  |..I....j...i....|

00000120  b4 9c 0b e3 59 3c c7 da  13 af 0e 4c b5 24 fa 72  |....Y<.....L.$.r|

00000130  7d bf 89 36 c6 75 f3 a9  89 cc b6 a8 cd a8 64 45  |}..6.u........dE|

00000140  ee f5 a2 3e 78 d3 9f 4e  20 27 ca 90 7f d9 27 a1  |...>x..N '....'.|

00000150  46 88 90 4a 22 ea 19 e6  b8 16 e4 15 ed a9 81 d0  |F..J"...........|

00000160  fa c9 24 a1 d5 75 fa 29  dd d9 65 31 5f f8 c3 c0  |..$..u.)..e1_...|

00000170  18 3b 3b 80 63 0f 8b ab  ef e5 b2 9b 07 97 9c 5f  |.;;.c.........._|

00000180  b4 35 bc de 75 aa f2 70  7e b1 5f 11 14 4f 7b 7f  |.5..u..p~._..O{.|

00000190  6e dc dc 72 62 80 59 3a  74 25 97 52 72 97 e1 15  |n..rb.Y:t%.Rr...|

000001a0  ad d0 f8 31 ff 92 68 d9  96 16 c1 54 b4 1d 1b 2e  |...1..h....T....|

000001b0  33 22 c7 8a 6a 93 ac dd  21 39 f8 6d 25 1f 04 37  |3"..j...!9.m%..7|

000001c0  71 1a c4 58 b8 39 7d 64  94 b4 70 b4 d9 77 cd 3a  |q..X.9}d..p..w.:|

000001d0  b5 a5 c0 62 5d 53 e5 66  4b 6f 39 c0 09 23 9b 25  |...b]S.fKo9..#.%|

000001e0  f9 91 53 55 b6 50 c8 f6  bf 5d 4d 20 ba 28 ca 68  |..SU.P...]M .(.h|

000001f0  02 9c 9a 8f ea f8 66 ca  08 7d 1e 88 46 95 c2 52  |......f..}..F..R|

00000200

Déchiffrement

À partir de là, seul le possesseur de la clé privée peut déchiffrer le message.

Tu peux utiliser la commande suivante pour déchiffrer le message :

openssl rsautl -decrypt -inkey private.pem -in encrypted_asymmetric.txt -out decrypted_asymmetric.txt

The command rsautl was deprecated in version 3.0. Use 'pkeyutl' instead.

Enter pass phrase for private.pem:

Tu dois alors entrer la passphrase de la clé privée.

Le fichier decrypted_asymmetric.txt contient le message déchiffré. Tu peux le lire avec la commande cat :

cat decrypted_asymmetric.txt 

Hello World!

Hash

Le hash est une fonction mathématique qui permet de transformer une donnée de taille variable en une donnée de taille fixe.
Le hash est une fonction à sens unique, c'est-à-dire qu'il est impossible de retrouver la donnée initiale à partir du hash,
sauf en essayant toutes les possibilités (brute force).

Les peuvent être utilisés pour vérifier l'intégrité d'un message, pour protéger un mot de passe dans une base de données...

De nombreux algorithmes de hash existent, les plus connus sont MD5, SHA1, SHA256, SHA512...
Calcul d'un hash

Pour cet exercice, tu vas devoir calculer le hash du fichier file.txt avec l'algorithme SHA256.

Commence par créer le fichier file.txt :

echo "Hello World!" > file.txt

Pour calculer le hash d'un fichier, tu peux utiliser la commande sha256sum :

sha256sum file.txt 

03ba204e50d126e4674c005e04d82e84c21366780af1f43bd54a37816b6ab340  file.txt

Si tu as créé le fichier file.txt tel que présenté plus haut, tu dois obtenir le même hash que ci-dessus.

Maintenant, modifie le fichier file.txt et recalcule le hash :

echo "1" >> file.txt

sha256sum file.txt

7b47db89827524567b0df2e8ca631a5e0bb85573b4463959834f79e0ed318083  file.txt

Comme tu peux le voir, le hash est complètement différent, même si tu n'as ajouté qu'un caractère.
Cela est dû au fait que le hash est sensible à la moindre modification, même minime.

Tu peux stocker le hash dans un fichier avec la commande > (cela nous servira par la suite pour vérifier l'intégrité d'un fichier) :

sha256sum file.txt > file.sha256

Il est également possible de calculer le hash d'une chaîne de caractères directement avec la commande echo :

echo -n "Hello World!" | sha256sum

7f83b1657ff1fc53b92dc18148a1d65dfc2d4b1fa3d677284addd200126d9069  -

L'option -n de la commande echo permet de ne pas ajouter de retour à la ligne à la fin de la chaîne de caractères.
Vérification d'un hash

Pour vérifier qu'un fichier a bien un hash donné, tu peux utiliser la commande sha256sum :

sha256sum -c file.sha256

file.txt: Réussi

Cette commande permet de vérifier que le fichier file.txt a bien le hash contenu dans le fichier file.sha256.

C'est pour cette raison que lors du téléchargement d'un fichier, il est souvent possible de télécharger le fichier et son hash.
Cela permet de vérifier que le fichier téléchargé n'a pas été modifié.
Les mots de passe

Pour une application web, les mots de passe sont souvent stockés dans une base de données. Cependant, il est important
de ne pas stocker les mots de passe en clair, car si la base de données est compromise, les mots de passe seront accessibles
à tous.

Pour cela, on stocke le hash du mot de passe dans la base de données. Lors de la connexion d'un utilisateur, on calcule le hash
du mot de passe entré par l'utilisateur et on le compare au hash stocké dans la base de données. Si les deux hash sont identiques,
alors le mot de passe est correct.

Exemple en PHP :

<?php

$userPasswod = $_POST['password'];

$databasePassword = '$2a$10$ZmuWF64FHtSTTA0EPR6ChOOdEU1MPXNDQSmxWdS3OuG4iX3zBmZXC';

$isPasswordCorrect = password_verify($userPasswod, $databasePassword);

Dans cet exemple, la variable $databasePassword contient le hash du mot de passe de l'utilisateur. La fonction password_verify permet
de vérifier que le mot de passe entré par l'utilisateur est correct en comparant le hash du mot de passe entré avec le hash stocké dans la base de données.

L'algorithme de hash utilisé ici est bcrypt, qui est un algorithme de hash sécurisé.
Bruteforce

Comme on l'a vu, il est impossible de retrouver la donnée initiale à partir du hash. Cependant, il est possible d'essayer toutes les possibilités
pour trouver la donnée initiale. C'est ce qu'on appelle une attaque par bruteforce.

Par exemple, pour un mot de passe volé dans une base de données et stocké en md5, il est possible de tester toutes les combinaisons de caractères
pour trouver le mot de passe initial. Cela est possible car l'algorithme de hash md5 est très rapide à calculer, ce qui permet de tester un grand nombre.
Sécurité des algorithmes de hash

Certains algorithmes de hash sont considérés comme non sécurisés, c'est-à-dire qu'il est possible de trouver une collision,
c'est-à-dire deux données différentes qui ont le même hash. C'est le cas de MD5 et SHA1. De plus, ces hash sont vulnérables
à des attaques par dictionnaire, autrement dit qu'il est possible de trouver une donnée qui a un hash donné en cherchant dans
un dictionnaire de données précalculées. Cela est dû au fait que ces algorithmes de hash sont très rapides à calculer.

Les algorithmes de hash considérés comme sécurisés sont par exemple bcrypt ou argon2. Ces algorithmes sont plus lents
à calculer, ce qui les rend plus résistants aux attaques par dictionnaire. Ils intègrent également un sel, c'est-à-dire
une donnée aléatoire qui est ajoutée à la donnée initiale avant le calcul du hash. Cela permet de rendre le bruteforce plus
difficile.
💪 Conclusion

Valide l'atelier si tu as réussi à chiffrer et déchiffrer le message, et à calculer et vérifier le hash du fichier file.txt.
