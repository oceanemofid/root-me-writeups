# Install files — Web Serveur, ch6 (15 points)

## Objectif
Trouver une ressource sensible laissée accessible après le déploiement de l'application.

## Reconnaissance
La page de base (`http://challenge01.root-me.org/web-serveur/ch6/`) ne contient aucun lien direct exploitable, mais son code source HTML contient un commentaire indiquant la présence d'un CMS (phpBB) installé sur le serveur.

## Vulnérabilité identifiée
**Répertoire d'installation d'un CMS laissé accessible en production.** Les scripts d'installation (`/install/`, `/setup/`) de nombreux CMS doivent être supprimés après la mise en service ; s'ils restent présents, ils peuvent permettre de reconfigurer l'application ou de divulguer des informations sensibles.

## Exploitation
1. Lecture du commentaire dans le code source de la page de base, indiquant un répertoire `/phpbb/`.
2. Navigation vers `http://challenge01.root-me.org/web-serveur/ch6/phpbb/`.
3. La page affiche le message de validation du challenge.

## Résultat
Challenge validé ✅

## Remédiation
- Supprimer systématiquement les répertoires d'installation/configuration après déploiement.
- Ne jamais laisser de commentaires révélant l'arborescence interne du serveur dans le code livré au client.
