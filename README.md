# INFO001_DESBOS_GRIVA
TP1 sécurité des applications

Alexandre Desbos
Baptiste Griva

## Question 1

Pour réaliser un couple de clé publique/privée RSA:
    
    1. Choisir deux grand nombre premier p et q (1024 bits)
    2. calculer n = p x q et z = (p-1)x(q-1)
    3. Choisir un nombre e tel que e et z soient premiers entre eux (aucun facteur premier commun)
    4. Trouver d tel que e x d ≡ 1 mod(z). Il faut trouver d tel que lorsqu'on divise (e x d) par z on doit avoir comme rester 1

## Question 2

Chiffrement d'un message M en RSA:

    Avec la clé publique du destinataire (e, n):
    C = M^e mod(n) -> avec "e" l'exposant public et "n" le module

Déchiffrement du message C:

    Avec la clé privée du destinataire (n, d):
    M = C^d mod(n)

## Question 3

Les 4 informations importantes contenues dans un certificat sont:

    1. L'autorité de certification
    2. Le nom de domaine de l'entité certifié (CN)
    3. La clé publique
    4. La signature

## Question 4

Pour l'authentification du site de Alice, il faut:

    1. Récupérer la clé publique de l'autorité de certification (ca1)
    2. Calculer : h1 = DKPUBca1(signature)
                  h2 = hash(infos-certif)
    3. Comparer h1 et h2
    4. Vérifier qu'un des noms DNS correpsond au nom d'utilisateur saisi dans l'URL
    5. Le certificat est validé, il faut maintenant réaliser l'authentification du serveur en envoyant un challenge que le serveur va chiffrer avec la clé privée associé à la clé publique.

## 4.1 Génération de clé rsa

Pour la génération de la clé rsa, on utilise le commmande genrsa

 $ openssl genrsa -out rsa_keys.pem 512

## Question 5

La longueur des nombres premiers est de 256 bits chacun
La longueur du n est de 512 bits

Le publicExponent est utilisé pour le chiffrement du message
Le privateExponent est utilisé pour le déchiffrement du message chiffré

Le publicExponent n'est pas difficile à deviner pour un pirate mais ca ne permet pas de compromettre la clé privée.

## Question 6

Chiffré une clé publique n'a pas d'interêt car elle à pour but d'être utilisé par tous les utilisateurs qui souhaite chiffré un message.

Chiffré une clé privé est important car cette clé doit être protégé. Elle permet de déchiffrer tous les message chiffré avec la clé publique.

## Question 7

L'encodage Base64 est utilisé.
Il a plusieurs avantage:
    -  L'encodage Base64 garantit que la clé peut être transférée à travers des systèmes ou des protocoles qui ne supportent que les caractères ASCII, tels que les e-mails ou certains protocoles de réseau.
    - Le format Base64 est plus lisible que les données binaires brutes, ce qui facilite certaines opérations de diagnostic, d'échange et de manipulation.


## Question 8

On retrouve bien les éléments attendu dans pub.pem:
    - Le modulus
    - L'exposant public

Disposer d'un fichier à part contenant uniquement la clé publique améliore la sécurité de la clé privée, on peut la stocké ailleurs. C'est donc plus simple de partagé la clé public.

## Question 9

Pour chiffrer un message RSA, il faut utiliser la clé publique du destinataire pour chiffrer le message. 
Cela garantit que seul le destinataire, qui possède la clé privée correspondante pourra déchiffrer le message.

## Question 10

    $ openssl pkeyutl -encrypt -in clair.txt -pubin -inkey pub.griva.pem -out cypher.bin

    - encrypt : Spécifie que l'opération est un chiffrement.
	- in clair.txt : Spécifie le fichier d'entrée contenant le message en clair que vous souhaitez chiffrer.
	- pubin : Indique que le fichier clé fourni avec l'option -inkey est une clé publique.
    - inkey pub.pem : Spécifie le fichier contenant la clé publique à utiliser pour le chiffrement.
	- out cipher.bin : Spécifie le fichier de sortie dans lequel le message chiffré sera enregistré.

## Question 11

En chiffrant plusieurs fois le même message, on voit que les résultats sont différent.
C'est normal car on ajoute un champ aléatoire devant les données à transmettre avant de les chiffrer.

## Question 12

L'option -showcerts demande au client d'afficher l'ensemble des certificats renvoyés par le serveur lors de l'établissement d'une connexion SSL/TLS. 
Le certificat du serveur lui-même, et tous les certificats intermédiaires nécessaires pour établir la chaîne de confiance jusqu'à une autorité de certification.

Le serveur à renvoyés 3 certificats:
	1.	celui émis pour “*.univ-grenoble-alpes.fr”.
	2.	Celui émis par “Sectigo RSA Organization Validation Secure Server CA”.
	3.	Celui “USERTrust RSA Certification Authority”.

## Question 13