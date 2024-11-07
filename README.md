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
    4. 


## Question 4

Pour l’authentification du site de Alice, il faut:

    1. Récupérer la clé publique de l'autorité de certification (ca1)
    2. Calculer : h1 = DKPUBca1(signature)
                  h2 = hash(infos-certif)
    3. Comparer h1 et h2
    4. Vérifier qu'un des noms DNS correpsond au nom d'utilisateur saisi dans l'URL
    5. Le certificat est validé, il faut maintenant réaliser l'authentification du serveur en envoyant un challenge que le serveur va chiffer avec la clé privée associé à la clé publique.


## 4.1 Génération de clé rsa

Pour la génération de la clé rsa, on utilise le commmande genrsa

$ openssl genrsa -out rsa_keys.pem 512



