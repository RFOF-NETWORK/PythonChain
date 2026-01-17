# PythonChain
InterBOxSpiderWeb.NET PRVPNRFAI.py 2025 - 2029

---

README.md

PythonChain – Die erste vollständig in Python implementierte dezentrale Blockchain

PythonChain ist eine von Grund auf in reinem Python entwickelte Blockchain‑Infrastruktur.  
Ziel ist es, eine offene, transparente und vollständig nachvollziehbare Referenz‑Blockchain zu schaffen, die ohne externe Tools auskommt und Entwicklern echte Dezentralität, Unabhängigkeit und Erweiterbarkeit bietet.

Dieses Repository enthält:

- eine vollständige Blockchain‑Implementierung (Block, Chain, Proof‑of‑Work, Validierung),
- eine Node‑Registry für ein einfaches P2P‑Netzwerk,
- eine Flask‑API zur Interaktion,
- ein leichtes HTML/CSS/JS‑Frontend,
- eine klare, modulare Projektstruktur für Weiterentwicklung und Forschung.

---

📁 Projektstruktur

```
pythonchain/
├─ README.md
├─ requirements.txt
├─ pythonchain/
│  ├─ init.py
│  ├─ block.py
│  ├─ blockchain.py
│  ├─ node.py
│  ├─ api.py
│  └─ config.py
└─ frontend/
   ├─ index.html
   ├─ style.css
   └─ app.js
```

Backend (pythonchain/)
Enthält die komplette Blockchain‑Logik:

| Datei | Beschreibung |
|-------|--------------|
| block.py | Definition der Blockstruktur (Index, Timestamp, Daten, Hash, Nonce). |
| blockchain.py | Verwaltung der Chain, Mining (PoW), Validierung, Transaktionen. |
| node.py | Node‑Registry für einfache Dezentralisierung & Block‑Broadcasting. |
| api.py | Flask‑API für Mining, Transaktionen, Chain‑Abfrage, Node‑Registrierung. |
| config.py | Konfiguration (z. B. Port, Difficulty). |

Frontend (frontend/)
Ein leichtes, unabhängiges UI:

| Datei | Beschreibung |
|-------|--------------|
| index.html | Minimalistisches Interface für Interaktion mit der API. |
| style.css | Einfaches, dunkles Theme. |
| app.js | JavaScript‑Logik für Transaktionen, Mining & Chain‑Anzeige. |

---

🚀 Features

Blockchain‑Kern
- Vollständige Blockstruktur  
- SHA‑256 Hashing  
- Proof‑of‑Work Mining  
- Transaktionspool  
- Validierungslogik  

Dezentralisierung
- Node‑Registrierung  
- Broadcast‑Mechanismus  
- Mehrere Instanzen auf verschiedenen Ports  

API‑Endpunkte
| Route | Methode | Beschreibung |
|-------|---------|--------------|
| /transactions/new | POST | Neue Transaktion hinzufügen |
| /mine | POST | Neuen Block minen |
| /chain | GET | Vollständige Blockchain abrufen |
| /nodes/register | POST | Neue Nodes registrieren |

Frontend
- Transaktionen erstellen  
- Mining auslösen  
- Chain anzeigen  

---

🛠 Installation

1. Repository klonen

`bash
git clone https://github.com/DEIN_USERNAME/pythonchain.git
cd pythonchain
`

2. Virtuelle Umgebung (optional, empfohlen)

`bash
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows
`

3. Abhängigkeiten installieren

`bash
pip install -r requirements.txt
`

---

▶️ Backend starten

`bash
python -m pythonchain.api
`

Standardport: 5000

---

🌐 Frontend öffnen

Einfach die Datei öffnen:

`
frontend/index.html
`

oder lokal hosten:

`bash
cd frontend
python -m http.server
`

---

📡 Mehrere Nodes starten

Beispiel:

`bash

Node 1
python -m pythonchain.api

Node 2 (anderer Port)
set NODE_PORT=5001
python -m pythonchain.api
`

Nodes registrieren:

`bash
curl -X POST http://localhost:5000/nodes/register \
     -H "Content-Type: application/json" \
     -d "{\"nodes\": [\"http://localhost:5001\"]}"
`

---

🎯 Ziel & Vision

PythonChain soll:

- eine vollständig transparente Python‑Blockchain sein,
- Entwicklern echte Dezentralität ermöglichen,
- ohne Drittanbieter‑Tools funktionieren,
- als Referenzimplementierung dienen,
- langfristig in das Python‑Ökosystem integriert werden können.

---

📄 Lizenz

Dieses Projekt ist offen für Forschung, Entwicklung und Weiterverwendung.  
Lizenz kann nach Bedarf ergänzt werden.

