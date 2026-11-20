# JWT - Introduction — Web Serveur (20 points)

## Objectif
Obtenir un accès admin en forgeant un JSON Web Token (JWT) sans connaître la clé de signature.

## Reconnaissance
En se connectant en tant que `guest`, un cookie `jwt` apparaît (observé avec Burp Suite). Un JWT est composé de trois parties séparées par des points et encodées en base64 : un **header** (type + algorithme de signature), un **payload** (les données, ici le username) et une **signature**. Le décodage du token révèle un algorithme `HS256` et un payload contenant `username: guest`.

## Vulnérabilité identifiée
**Acceptation de l'algorithme `none` par le serveur.** Un JWT dont le header annonce `alg: none` n'a pas de signature du tout : si le serveur ne rejette pas explicitement cette valeur et continue de faire confiance au contenu du payload, n'importe qui peut forger un token valide sans connaître le secret de signature.

## Exploitation
Utilisation de l'outil [`jwt_tool`](https://github.com/ticarpi/jwt_tool) en mode interactif (tampering) :
1. Modification du header : `alg` passé de `HS256` à `none`.
2. Modification du payload : `username` passé de `guest` à `admin`.
3. Signature : choix de l'option « Strip signature using the none algorithm ».

jwt_tool génère alors le nouveau jeton forgé, qu'il suffit de substituer au cookie `jwt` d'origine dans Burp Suite avant de recharger la page.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Toujours vérifier la signature d'un JWT côté serveur, et rejeter explicitement tout token annonçant `alg: none`.
- Whitelister les algorithmes de signature acceptés plutôt que de faire confiance à la valeur `alg` fournie par le client.
