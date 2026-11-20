# HTTP - User-agent — Web Serveur (10 points)

## Objectif
Devenir « admin » aux yeux de l'application pour obtenir le mot de passe de validation.

## Reconnaissance
La page affiche le message : *« Wrong user-agent: you are not the admin browser! »*. Elle vérifie donc une information envoyée par le navigateur pour décider si l'on est autorisé.

## Vulnérabilité identifiée
**Contrôle d'accès basé sur le header HTTP `User-Agent`.** Ce header est entièrement fourni par le client et peut contenir n'importe quelle valeur — il ne prouve rien sur l'identité réelle du visiteur.

## Exploitation
1. Ouverture des DevTools (`F12`).
2. Modification du header `User-Agent` envoyé par le navigateur pour qu'il corresponde à celui attendu par l'application (simulant le « navigateur admin »).
3. Rechargement de la page : le message d'erreur disparaît et le mot de passe de validation s'affiche.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Ne jamais baser une décision d'autorisation sur un header HTTP non vérifiable/signé par le serveur.
- Utiliser une vraie authentification (session, token) pour distinguer les rôles.
