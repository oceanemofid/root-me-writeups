# HTML - Code source — Web Serveur (5 points)

## Objectif
Le challenge demande de trouver un moyen de contourner l'authentification d'une page de connexion sans connaître le mot de passe à l'avance.

## Reconnaissance
La page présente un formulaire de login classique (champ identifiant + mot de passe), sans aucune autre fonctionnalité visible.

## Vulnérabilité identifiée
**Exposition d'information sensible dans le code source HTML.** Le développeur a laissé le mot de passe en clair dans un commentaire HTML (`<!-- ... -->`). Un commentaire HTML est invisible à l'écran mais reste entièrement lisible par quiconque consulte le code source de la page — il n'est jamais « caché » côté client.

## Exploitation
1. Clic droit sur la page → *Afficher le code source* (ou `Ctrl+U`).
2. Repérage du commentaire contenant le mot de passe en clair.
3. Saisie du mot de passe dans le formulaire de connexion.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Ne jamais laisser de secret (mot de passe, clé, token) dans le HTML livré au navigateur, y compris en commentaire.
- Les commentaires de debug/dev doivent être retirés avant mise en production (build de prod, linter, revue de code).
