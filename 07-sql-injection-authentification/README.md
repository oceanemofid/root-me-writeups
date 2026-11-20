# SQL injection - Authentification — Web Serveur (30 points)

## Objectif
Contourner le formulaire d'authentification sans connaître d'identifiants valides.

## Reconnaissance
Un formulaire de login classique (champ login + mot de passe), dont la requête SQL côté serveur est vraisemblablement construite par concaténation directe des champs du formulaire.

## Vulnérabilité identifiée
**Injection SQL dans la requête d'authentification.** La requête ressemble à :
```sql
SELECT * FROM users WHERE login='$login' AND password='$password'
```
Sans échappement ni requête préparée, il est possible d'injecter du SQL via les champs du formulaire.

## Exploitation
Deux approches testées :
- `' OR '1'='1` : rend la condition toujours vraie, renvoie potentiellement la première ligne de la table (générique, ne cible personne en particulier).
- `admin'--` dans le champ login : ferme la chaîne après le login `admin`, puis `--` commente la suite de la requête (y compris la vérification du mot de passe). La requête devient effectivement `SELECT * FROM users WHERE login='admin'--...`, ce qui authentifie en tant qu'`admin` sans connaître son mot de passe — contrairement au `OR 1=1` générique, cette technique cible précisément un compte connu.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Utiliser systématiquement des requêtes préparées (prepared statements / requêtes paramétrées), jamais de concaténation de chaînes dans une requête SQL.
- Appliquer le principe de moindre privilège sur le compte de base de données utilisé par l'application.
