# Fichier de sauvegarde — Web Serveur, ch11 (15 points)

## Objectif
Trouver les identifiants de connexion permettant d'accéder à la zone protégée.

## Reconnaissance
Une page de login classique à l'URL `http://challenge01.root-me.org/web-serveur/ch11/`, sans indice visible directement sur la page.

## Vulnérabilité identifiée
**Divulgation d'information via un fichier de sauvegarde d'éditeur de texte.** De nombreux éditeurs (type Vim) créent automatiquement une copie de sauvegarde du fichier source lors de l'édition (suffixe `~`, `.bak`, `.swp`). Si ce fichier reste accessible sur le serveur, il permet de lire le code source PHP de la page, normalement interprété et donc invisible.

## Exploitation
1. Tentative d'accès à `index.php~` (copie de sauvegarde du fichier source).
2. Le serveur sert ce fichier tel quel (texte brut, non interprété par PHP), révélant le code source complet, y compris des identifiants codés en dur :
   ```php
   $username = "ch11";
   $password = "[FLAG_REDACTED]";
   ```
3. Utilisation de ces identifiants sur le formulaire de login.

## Résultat
Challenge validé ✅

## Remédiation
- Configurer le serveur web pour refuser explicitement l'accès aux fichiers de sauvegarde (`.bak`, `~`, `.swp`, `.old`...).
- Ne jamais coder des identifiants en dur dans le code source, même sur un environnement de test : même sans fuite de fichier, un accès au dépôt de code (ex: `.git` exposé) aurait le même effet.
