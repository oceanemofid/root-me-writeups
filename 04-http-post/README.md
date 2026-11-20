# HTTP - POST (RandGame) — Web Serveur (15 points)

## Objectif
Obtenir un score suffisamment élevé pour valider le challenge, alors que le jeu ne permet normalement d'obtenir qu'un score aléatoire limité.

## Reconnaissance
Une page de « jeu » avec un champ `score` caché dans un formulaire, soumis en `POST`. Un gestionnaire `onsubmit` en JavaScript tente de contrôler/valider la valeur avant l'envoi.

## Vulnérabilité identifiée
**Validation du score effectuée uniquement côté client.** Le serveur fait confiance à la valeur du champ `score` envoyée en `POST`, sans la recalculer ni la vérifier lui-même.

## Exploitation
Modifier manuellement la valeur du champ dans les DevTools (Elements) et retirer le `onsubmit` ne suffit pas de façon fiable (la modification peut être perdue avant l'envoi réel). La méthode fiable est d'utiliser la Console JavaScript pour forger la requête :

```js
document.forms[0].score.value = 9999;
document.forms[0].onsubmit = null;
document.forms[0].submit();
```

Cela neutralise la vérification côté client puis soumet directement un score élevé au serveur.

*(Root-Me propose aussi les méthodes équivalentes via `curl` ou Postman pour rejouer la requête POST avec un score modifié.)*

## Résultat
Challenge validé ✅ (mot de passe masqué ici : `[FLAG_REDACTED]`)

## Remédiation
- Toujours revalider/recalculer côté serveur toute donnée métier critique (score, prix, quantité...), même si elle est aussi calculée côté client.
- Ne jamais faire confiance à une donnée POST venant du client sans contrôle serveur.
