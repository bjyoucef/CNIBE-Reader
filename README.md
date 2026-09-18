# CNIBE & Passport Reader — Lecture NFC de titres d'identité biométriques (eMRTD)

**Français** | [English](README_EN.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![ICAO Doc 9303](https://img.shields.io/badge/ICAO-Doc%209303-orange.svg)](https://www.icao.int/)

Architecture client/serveur pour la lecture sans contact (NFC) des **Cartes Nationales d'Identité Biométriques (CNIBE, format TD1)** et **Passeports Biométriques (format TD3)**, conforme aux normes **ICAO Doc 9303** et **ISO 7816-4**. Testé sur cartes et passeports algériens.

## ✨ Fonctionnalités

- 📡 Lecture NFC via lecteurs PC/SC (ex. Identiv uTrust, ACS ACR122U)
- 🔐 Protocole BAC (Basic Access Control) + parsing ASN.1 / EF.DG1, DG2 (photo), DG7 (signature), DG11
- 🖥️ Agent local HTTP (`127.0.0.1:5001`) : lecture puce + jeton + adresse officielle
- 🌐 Serveur Flask central (`0.0.0.0:5000`) : affichage et centralisation multi-postes en LAN
- 🧪 Tests unitaires intégrés (vecteurs MRZ TD1/TD3 fictifs)

## 📁 Organisation du projet

```
CNIBE-Reader/
├── LICENSE                             # Licence MIT
├── README.md                           # Ce fichier (FR)
├── README_EN.md                        # Documentation (EN)
│
├── local/                              # POSTE CLIENT (opérateur + lecteur USB)
│   ├── cnibe_agent.py                  # Passerelle HTTP locale (127.0.0.1:5001)
│   ├── read_cnibe_safe.py              # Moteur PC/SC passif (ICAO BAC / ASN.1)
│   ├── get_card_token.ps1              # Pont 32-bit vers le composant officiel
│   ├── tokens_cache.example.json       # Exemple de structure du cache (à copier en local)
│   ├── requirements.txt                # Dépendances (pyscard, pycryptodome)
│   ├── lancer_agent.bat                # Lanceur 1-clic Windows
│   └── README_CLIENT.md                # Guide d'installation client
│
├── serveur/                            # SERVEUR WEB CENTRAL (LAN)
│   ├── server_minimal.py               # Application Flask (0.0.0.0:5000)
│   ├── templates/index.html            # Interface web (formulaire, photo, signature)
│   ├── requirements.txt                # Dépendances (Flask, requests)
│   ├── lancer_serveur.bat              # Lanceur 1-clic Windows
│   └── README_SERVEUR.md               # Guide d'administration
│
├── .dev/                               # OUTILS DEV & DIAGNOSTIC (hors production)
│   ├── inspect_endpoints.py            # Énumération des endpoints publics
│   ├── inspect_live_ministere.py       # Diagnostic HTTP (valeurs fictives, à renseigner en local)
│   ├── uid.py                          # Test détection lecteur/carte
│   └── README.md                       # Doc des scripts de dev
│
└── .gitignore                          # Protection des données sensibles
```

> ℹ️ `local/tokens_cache.json` (vrai cache) et `local/DzaEidCard.msi` (installeur officiel propriétaire) sont **volontairement exclus** du dépôt. Copiez `tokens_cache.example.json` vers `tokens_cache.json` en local, et récupérez le composant officiel depuis sa source officielle.

## 🧰 Prérequis

- **Matériel :** lecteur NFC PC/SC USB + carte de test **avec consentement du porteur**
- **Logiciel :** Windows + Python 3.10/3.11, service *Carte à puce* actif
- **Réseau :** port 5000/TCP ouvert entre clients et serveur (LAN)

## 🚀 Démarrage rapide

**1. Poste client (lecteur USB branché) :**
```powershell
cd local
py -3.11 -m pip install -r requirements.txt
# ou double-clic sur lancer_agent.bat
```
L'agent écoute sur `http://127.0.0.1:5001`.

**2. Serveur central :**
```powershell
cd serveur
py -3.11 -m pip install -r requirements.txt
# ou double-clic sur lancer_serveur.bat
```
Le serveur écoute sur `http://0.0.0.0:5000` (ex. `http://192.168.1.50:5000` sur le LAN).

**3. Utilisation :** ouvrez l'adresse du serveur dans le navigateur, posez la carte sur le lecteur, cliquez **Lire la Carte (NFC)**. Les données sont collectées localement puis transmises au serveur.

## 🔒 Confidentialité & sécurité

- Aucune photo, signature, MRZ, NIN, jeton ou log réel n'est versionné (voir `.gitignore`).
- Les vecteurs de test dans le code et les scripts `.dev/` sont **fictifs** (`XXXXXXXXX`, `JJ/MM/AAAA`, nom générique).
- Ne commitez jamais de données de vraies cartes.

## ⚖️ Avertissement légal

**Usage légal uniquement, avec le consentement explicite du porteur de la carte.**

- N'utilisez cet outil que sur **vos propres documents** ou avec l'**autorisation écrite** du titulaire, conformément à la loi algérienne 18-07 sur la protection des données personnelles et à toute réglementation applicable.
- Les scripts `.dev/` interrogeant `macnibe.interieur.gov.dz` sont fournis à but **pédagogique / diagnostic** : usage modéré, sans contournement de sécurité, sans stockage de données de tiers.
- L'auteur décline toute responsabilité en cas d'usage abusif ou illégal.

## 📄 Licence

Projet sous licence **MIT** — voir [LICENSE](LICENSE).

## 👤 Auteur & Contact

**Youcef Badjadi (bjyoucef)**
- **GitHub :** [@bjyoucef](https://github.com/bjyoucef)
- **Dépôt :** [https://github.com/bjyoucef/CNIBE-Reader](https://github.com/bjyoucef/CNIBE-Reader)
