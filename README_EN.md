# CNIBE & Passport Reader — NFC reader for biometric ID documents (eMRTD)

[Français](README.md) | **English**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![ICAO Doc 9303](https://img.shields.io/badge/ICAO-Doc%209303-orange.svg)](https://www.icao.int/)

Client/server architecture for contactless (NFC) reading of **biometric National ID Cards (CNIBE, TD1 format)** and **biometric Passports (TD3 format)**, compliant with **ICAO Doc 9303** and **ISO 7816-4**. Tested on Algerian cards and passports.

## ✨ Features

- 📡 NFC reading via PC/SC readers (e.g. Identiv uTrust, ACS ACR122U)
- 🔐 BAC (Basic Access Control) + ASN.1 parsing / EF.DG1, DG2 (photo), DG7 (signature), DG11
- 🖥️ Local HTTP agent (`127.0.0.1:5001`): chip reading + token + official address lookup
- 🌐 Central Flask server (`0.0.0.0:5000`): display and multi-workstation centralization over LAN
- 🧪 Built-in unit tests (fictitious TD1/TD3 MRZ vectors)

## 📁 Project layout

```
CNIBE-Reader/
├── LICENSE                             # MIT License
├── README.md                           # Documentation (FR)
├── README_EN.md                        # This file (EN)
│
├── local/                              # CLIENT WORKSTATION (operator + USB reader)
│   ├── cnibe_agent.py                  # Local HTTP gateway (127.0.0.1:5001)
│   ├── read_cnibe_safe.py              # Passive PC/SC engine (ICAO BAC / ASN.1)
│   ├── get_card_token.ps1              # 32-bit bridge to the official component
│   ├── tokens_cache.example.json       # Cache structure example (copy locally)
│   ├── requirements.txt                # Dependencies (pyscard, pycryptodome)
│   ├── lancer_agent.bat                # 1-click Windows launcher
│   └── README_CLIENT.md                # Client setup guide (FR)
│
├── serveur/                            # CENTRAL WEB SERVER (LAN)
│   ├── server_minimal.py               # Flask app (0.0.0.0:5000)
│   ├── templates/index.html            # Web UI (form, photo, signature)
│   ├── requirements.txt                # Dependencies (Flask, requests)
│   ├── lancer_serveur.bat              # 1-click Windows launcher
│   └── README_SERVEUR.md               # Admin guide (FR)
│
├── .dev/                               # DEV & DIAGNOSTIC TOOLS (not for production)
│   ├── inspect_endpoints.py            # Public endpoints enumeration
│   ├── inspect_live_ministere.py       # HTTP diagnostics (dummy values, fill in locally)
│   ├── uid.py                          # Reader/card detection test
│   └── README.md                       # Dev scripts documentation
│
└── .gitignore                          # Sensitive-data protection rules
```

> ℹ️ `local/tokens_cache.json` (real cache) and `local/DzaEidCard.msi` (proprietary official installer) are **deliberately excluded** from the repository. Copy `tokens_cache.example.json` to `tokens_cache.json` locally, and obtain the official component from its official source.

## 🧰 Requirements

- **Hardware:** PC/SC USB NFC reader + test card **with the holder's consent**
- **Software:** Windows + Python 3.10/3.11, *Smart Card* service running
- **Network:** TCP port 5000 open between clients and server (LAN)

## 🚀 Quick start

**1. Client workstation (USB reader plugged in):**
```powershell
cd local
py -3.11 -m pip install -r requirements.txt
# or double-click lancer_agent.bat
```
The agent listens on `http://127.0.0.1:5001`.

**2. Central server:**
```powershell
cd serveur
py -3.11 -m pip install -r requirements.txt
# or double-click lancer_serveur.bat
```
The server listens on `http://0.0.0.0:5000` (e.g. `http://192.168.1.50:5000` on the LAN).

**3. Usage:** open the server address in the browser, place the card on the reader, click **Lire la Carte (NFC)**. Data is collected locally, then sent to the server.

## 🔒 Privacy & security

- No real photo, signature, MRZ, NIN, token or log is versioned (see `.gitignore`).
- Test vectors in the code and `.dev/` scripts are **fictitious** (`XXXXXXXXX`, `JJ/MM/AAAA`, generic name).
- Never commit data from real cards.

## ⚖️ Legal notice

**Lawful use only, with the explicit consent of the card holder.**

- Use this tool only on **your own documents** or with the **written authorization** of the holder, in compliance with Algerian law 18-07 on personal data protection and any applicable regulation.
- The `.dev/` scripts querying `macnibe.interieur.gov.dz` are provided for **educational / diagnostic purposes**: moderate use, no security bypass, no third-party data storage.
- The author disclaims all liability for abusive or unlawful use.

## 📄 License

Released under the **MIT** License — see [LICENSE](LICENSE).

## 👤 Author & Contact

**Youcef Badjadi (bjyoucef)**
- **GitHub:** [@bjyoucef](https://github.com/bjyoucef)
- **Repository:** [https://github.com/bjyoucef/CNIBE-Reader](https://github.com/bjyoucef/CNIBE-Reader)
