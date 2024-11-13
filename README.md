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
Il a plusieurs avantage: - L'encodage Base64 garantit que la clé peut être transférée à travers des systèmes ou des protocoles qui ne supportent que les caractères ASCII, tels que les e-mails ou certains protocoles de réseau. - Le format Base64 est plus lisible que les données binaires brutes, ce qui facilite certaines opérations de diagnostic, d'échange et de manipulation.

## Question 8

On retrouve bien les éléments attendu dans pub.pem: - Le modulus - L'exposant public

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

Le serveur à renvoyés 3 certificats: 1. celui émis pour “\*.univ-grenoble-alpes.fr”. 2. Celui émis par “Sectigo RSA Organization Validation Secure Server CA”. 3. Celui “USERTrust RSA Certification Authority”.

## Question 13

x509 est un format standard pour certificats de clé publique

Le sujet : Subject: C = FR, ST = Auvergne-Rh\C3\B4ne-Alpes, O = Universit\C3\A9 Grenoble Alpes, CN = \*.univ-grenoble-alpes.fr

    - C (Country) : Indique le pays de l'organisation
    - ST (State) : Indique l'état
    - L (Locality Name) : Indique la localité / ville
    - O (Organization Name) : Indique le nom de l'organisation
    - CN (Common Name) : Le nom de domaine ou l'identité principale

## Question 14

Le champ s est le sujet du certificat.
Le champ i représente l'autorité qui a émis (signé) le certificat.

## Question 15

    1.	Le certificat contient la clé publique du serveur.
    2.	Les algorithmes utilisés sont indiqués dans le certificat. Ici, c'est RSA SHA-256
    3.	L'attribut CN contient le nom commun du certificat, dans ce cas, *.univ-grenoble-alpes.fr
    4.	Les autre domaines sont dans alternatives names: DNS:*.univ-grenoble-alpes.fr, DNS:univ-grenoble-alpes.fr
    5.	La période de validité est: 08 avril 2024 au 08 avril 2025
    6.	Le lien .crl correspond à une liste de révocation de certificats, pour vérifier si un certificat a été révoqué par l'autorité de certification avant sa date d'expiration.

## Question 16

    1.  Le certificat est signé par Sectigo RSA Organization Validation Secure Server CA
    2.  La formule de signature S = E<sub>KRPIV_CA</sub>(H(m)) avec :
        - H(m) : la valeur de hachage du message m avec SHA-256
        - Ed : L'opération de chiffrement avec la clé privée du signataire (le CA dans notre cas)
        - n : le module utilisé dans RSA

## Question 17

    1.  le certificat de la CA qui a délivré le certificat de de www.univ-grenoble-alpes.fr a pour sujet :
        - s:C = GB, ST = Greater Manchester, L = Salford, O = Sectigo Limited, CN = Sectigo RSA Organization Validation Secure Server CA
    2.  Sa taille de clé publique est de 2048 (bit)
    3.  Il a été signé par USERTrust RSA Certification Authority

## Question 18

    1.  Le certificat permettant de valider le certificat du dernier niveau est le certificat root :
        - USERTrust RSA Certification Authority
    2. Il se trouve dans le fichier des certificats de confiance de la machine :
        - ici /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt

## Question 19

    1.  Les champs subject et issuer sont les mêmes, le certificat est auto-signé car c'est un certificat racine.
    2.  La formule qui a permit de générer le certificat est : S = E~KPRIV-CA~(H(m))
    3.  Ce type de certificat est appelé certificat auto-signé.

## Question 20

La taille de la clé est de 4096 bits -> Public-Key: (4096 bit)
On voit que le subjet et le issuer sont identiques, ceux qui permet de savoir qu'il s'agit bien d'un certificat racine auto-signé.
    C = FR, ST = Savoie, L = Chambéry, O = TP Sécurité, CN = Root Lorne
Cette autorité peut être utilisé pour:
    - Digital Signature
    - Certificate Sign
    - CRL Sign

## Question 21

- On à mis la valeur /home/etudiant/ca dans le paramètre dir de CA_Default.
- La clé privée de la CA doit être stockée dans le répertoire /home/etudiant/ca/private sous le nom ca.key.pem.
- Le certificat de la CA doit être stocké dans le répertoire /home/camanager/ca/certs sous le nom ca.cert.pem.