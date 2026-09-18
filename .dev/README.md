# 🛠️ Dossier `.dev` (Outils et Scripts de Diagnostic)

Ce dossier regroupe les scripts d'investigation, d'analyse réseau et de développement qui ne sont pas nécessaires en production (ni sur les postes clients, ni sur le serveur web en exploitation normale).

## Fichiers archivés :
- `inspect_endpoints.py` : Script d'analyse et d'énumération des points d'accès web du ministère de l'Intérieur.
- `inspect_live_ministere.py` : Diagnostic en direct des requêtes HTTP vers macnibe.interieur.gov.dz.

## ⚖️ Avertissement légal (dossier sensible)

* Ces scripts interrogent le service public `macnibe.interieur.gov.dz` **à but pédagogique / diagnostic uniquement**.
* Utilisez-les uniquement avec **vos propres cartes de test et votre consentement**, de façon modérée (pas de scraping massif).
* **Ne commitez jamais de vraies données personnelles** (numéro de carte, dates de naissance/expiration, jetons, photos). Les valeurs dans `inspect_live_ministere.py` sont des placeholders fictifs (`XXXXXXXXX`, `JJ/MM/AAAA`) : remplacez-les localement, sans les publier.
- `scratch_org.py.bak` : Sauvegarde temporaire du code de test unitaire.
