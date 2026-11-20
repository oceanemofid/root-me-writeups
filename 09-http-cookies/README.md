# HTTP - Cookies — Web Serveur, ch7 (20 points)

## Objectif
Obtenir les droits « admin » pour accéder à une fonctionnalité réservée (consultation d'adresses sauvegardées).

## Reconnaissance
Le site propose une fonctionnalité de sauvegarde d'adresse. En tentant de consulter les adresses sauvegardées, un message indique qu'il faut être administrateur. Un cookie est visible dans les DevTools (onglet Application), contenant un rôle en clair (ex: `ch7=visiteur`).

## Vulnérabilité identifiée
**Autorisation basée sur un cookie stocké en clair côté client, sans vérification d'intégrité côté serveur.** Le rôle de l'utilisateur est entièrement déterminé par la valeur du cookie, que le client peut modifier librement.

## Exploitation
1. Ouverture des DevTools → onglet Application → Cookies.
2. Modification de la valeur du cookie `ch7` de `visiteur` à `admin`.
3. Rechargement de la page : l'accès admin est désormais accordé et le mot de passe de validation s'affiche dans la section des adresses sauvegardées.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Ne jamais stocker un rôle ou un droit en clair, modifiable, côté client.
- Utiliser un cookie de session signé/chiffré, où seul le serveur peut faire le lien entre l'identifiant de session et le rôle réel de l'utilisateur en base de données.
