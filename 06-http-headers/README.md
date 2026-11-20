# HTTP - Headers — Web Serveur (15 points)

## Objectif
Accéder à une fonctionnalité réservée à l'administrateur, contrôlée par un header HTTP personnalisé.

## Reconnaissance
La page refuse l'accès tant qu'un header HTTP spécifique (de type `Header-Rootme-Admin`) n'est pas présent dans la requête.

## Vulnérabilité identifiée
**Contrôle d'accès basé sur un header HTTP arbitraire côté client.** Comme pour le User-Agent, rien n'empêche un client d'envoyer n'importe quel header avec n'importe quelle valeur : ce n'est pas une preuve d'autorisation.

## Exploitation
1. Éditer le header puis « Edit and Resend » depuis l'onglet Network des DevTools s'est révélé peu fiable pour un header personnalisé (le comportement de rejeu de requête varie selon le navigateur et ne garantit pas l'envoi correct du header modifié).
2. Solution fiable : forger une toute nouvelle requête directement en JavaScript depuis la Console, avec le header voulu :
   ```js
   fetch(location.href, { headers: { "Header-Rootme-Admin": "1" } })
     .then(r => r.text())
     .then(console.log);
   ```
3. La réponse contient le mot de passe de validation.

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Ne jamais utiliser un header fourni par le client comme preuve d'autorisation.
- S'appuyer sur une authentification serveur réelle (session, token signé) pour distinguer les rôles.
