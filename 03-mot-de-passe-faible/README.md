# Mot de passe faible — Web Serveur (10 points)

## Objectif
S'authentifier sur un formulaire de login protégé par un mot de passe peu robuste.

## Reconnaissance
Un formulaire de connexion classique, sans autre indice visible dans la page.

## Vulnérabilité identifiée
**Politique de mot de passe trop faible.** Le mot de passe attendu fait partie d'une liste de mots de passe triviaux/courants (pas de complexité minimale imposée côté application), le rendant devinable rapidement sans outil d'attaque élaboré.

## Exploitation
1. Test de quelques mots de passe courants/évidents dans le champ prévu.
2. Un des essais fonctionne et donne accès à la suite / au mot de passe de validation.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Imposer une politique de mot de passe robuste (longueur, complexité).
- Mettre en place une limitation de tentatives (rate limiting / verrouillage de compte) pour rendre le brute force impraticable.
