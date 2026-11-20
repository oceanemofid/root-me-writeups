# SQL injection - String — Web Serveur, ch19 "CMS v0.02" (30 points)

## Objectif
Extraire les identifiants administrateur stockés en base de données via une injection SQL de type UNION.

## Reconnaissance
Le CMS propose une fonctionnalité de recherche (champ « Recherche »). La base de données backend est SQLite3 (confirmé par un message d'erreur explicite de l'application).

## Vulnérabilité identifiée
**Injection SQL en contexte chaîne de caractères, point d'entrée : le champ de recherche.** L'entrée utilisateur est insérée directement dans la requête SQL construite côté serveur, sans échappement ni requête préparée.

## Exploitation
1. **Confirmation de l'injection** : une simple quote (`'`) dans le champ Recherche casse la requête SQL (erreur SQLite visible), confirmant l'absence d'échappement.
2. **Détermination du nombre de colonnes** via `UNION SELECT` avec des `NULL` :
   ```sql
   ' UNION SELECT NULL, NULL-- -
   ```
   Résultat cohérent (pas d'erreur de type « nombre de colonnes différent ») → **2 colonnes** confirmées.
3. **Énumération du schéma** via la table système `sqlite_master` :
   ```sql
   ' UNION SELECT name, sql FROM sqlite_master WHERE type='table'-- -
   ```
   Révèle les tables existantes, dont `users(username TEXT, password TEXT, Year INTEGER)`.
4. **Extraction des identifiants** :
   ```sql
   ' UNION SELECT username, password FROM users-- -
   ```
   Renvoie le couple identifiant/mot de passe recherché.

## Résultat
Challenge validé ✅ (identifiants masqués ici : `[FLAG_REDACTED]`)

## Remédiation
- Utiliser systématiquement des requêtes préparées (prepared statements), y compris pour une simple fonctionnalité de recherche.
- Restreindre les permissions du compte applicatif sur la base de données (pas d'accès en lecture à `sqlite_master` ou aux tables sensibles si non nécessaire).
- Ne jamais afficher les messages d'erreur SQL détaillés à l'utilisateur final en production (ils facilitent grandement l'énumération).
