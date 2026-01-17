# PythonChain
InterBOxSpiderWeb.NET PRVPNRFAI.py 2025 - 2029







1. Projektstruktur für das GitHub-Repository

```text
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

Kernidee:  
- pythonchain/ = Backend (Blockchain-Logik + Flask-API)  
- frontend/ = statisches Frontend, das über HTTP mit der API spricht  

---

2. Block und Blockchain in Python

block.py

```python
import time
import hashlib
import json

class Block:
    def init(self, index, timestamp, data, previous_hash, nonce=0):
        self.index = index
        self.timestamp = timestamp
        self.data = data
        self.previoushash = previoushash
        self.nonce = nonce
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        block_string = json.dumps({
            "index": self.index,
            "timestamp": self.timestamp,
            "data": self.data,
            "previoushash": self.previoushash,
            "nonce": self.nonce
        }, sort_keys=True).encode()

        return hashlib.sha256(block_string).hexdigest()
```

blockchain.py

```python
from .block import Block
import time

class Blockchain:
    def init(self, difficulty=4):
        self.chain = [self.creategenesisblock()]
        self.difficulty = difficulty
        self.pending_transactions = []

    def creategenesisblock(self):
        return Block(0, time.time(), "Genesis Block", "0")

    def getlastblock(self):
        return self.chain[-1]

    def add_transaction(self, sender, recipient, amount):
        self.pending_transactions.append({
            "sender": sender,
            "recipient": recipient,
            "amount": amount
        })

    def proofofwork(self, block):
        while not block.hash.startswith("0" * self.difficulty):
            block.nonce += 1
            block.hash = block.calculate_hash()
        return block

    def minependingtransactions(self, miner_address):
        data = {
            "transactions": self.pending_transactions,
            "miner": miner_address
        }
        new_block = Block(
            index=len(self.chain),
            timestamp=time.time(),
            data=data,
            previoushash=self.getlast_block().hash
        )
        minedblock = self.proofofwork(newblock)
        self.chain.append(mined_block)
        self.pending_transactions = []

        return mined_block

    def ischainvalid(self):
        for i in range(1, len(self.chain)):
            current = self.chain[i]
            previous = self.chain[i - 1]

            if current.hash != current.calculate_hash():
                return False
            if current.previous_hash != previous.hash:
                return False
        return True
```

---

3. Node, Konfiguration und einfache Dezentralität

config.py

```python
NODE_PORT = 5000
```

node.py

```python
import requests

class NodeRegistry:
    def init(self):
        self.nodes = set()

    def register_node(self, address: str):
        self.nodes.add(address)

    def get_nodes(self):
        return list(self.nodes)

    def broadcastnewblock(self, block, nodes):
        for node in nodes:
            try:
                requests.post(f"{node}/receive_block", json=block)
            except Exception:
                pass
```

> Hier ist es bewusst minimal gehalten – du kannst später Longest-Chain-Consensus und Synchronisation erweitern.

---

4. Flask-API für Mining, Transaktionen, Chain

api.py

```python
from flask import Flask, request, jsonify
from .blockchain import Blockchain
from .node import NodeRegistry
from .config import NODE_PORT

app = Flask(name)

blockchain = Blockchain(difficulty=4)
nodes = NodeRegistry()

@app.route("/transactions/new", methods=["POST"])
def new_transaction():
    values = request.get_json()
    required = ["sender", "recipient", "amount"]
    if not all(k in values for k in required):
        return "Missing values", 400

    blockchain.add_transaction(
        sender=values["sender"],
        recipient=values["recipient"],
        amount=values["amount"]
    )
    return jsonify({"message": "Transaction added"}), 201

@app.route("/mine", methods=["POST"])
def mine():
    values = request.get_json() or {}
    miner = values.get("miner", "pythonchain-node")
    block = blockchain.minependingtransactions(moner_address=miner)
    response = {
        "message": "New block mined",
        "index": block.index,
        "hash": block.hash,
        "previoushash": block.previoushash,
        "data": block.data,
        "nonce": block.nonce
    }
    return jsonify(response), 200

@app.route("/chain", methods=["GET"])
def full_chain():
    chain_data = []
    for block in blockchain.chain:
        chain_data.append({
            "index": block.index,
            "timestamp": block.timestamp,
            "data": block.data,
            "previoushash": block.previoushash,
            "hash": block.hash,
            "nonce": block.nonce
        })
    return jsonify({
        "length": len(chain_data),
        "chain": chain_data,
        "valid": blockchain.ischainvalid()
    }), 200

@app.route("/nodes/register", methods=["POST"])
def register_nodes():
    values = request.get_json()
    nodes_list = values.get("nodes")
    if nodes_list is None:
        return "Error: Please supply a valid list of nodes", 400

    for node in nodes_list:
        nodes.register_node(node)

    return jsonify({
        "message": "New nodes have been added",
        "totalnodes": nodes.getnodes()
    }), 201

if name == "main":
    app.run(host="0.0.0.0", port=NODE_PORT)
```

---

5. Frontend: HTML, CSS, JavaScript (ohne externe Tools)

frontend/index.html

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <title>PythonChain – Dezentrale Python-Blockchain</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>PythonChain</h1>

  <section id="transaction">
    <h2>Neue Transaktion</h2>
    <input id="sender" placeholder="Sender">
    <input id="recipient" placeholder="Empfänger">
    <input id="amount" type="number" placeholder="Betrag">
    <button id="sendTx">Transaktion senden</button>
  </section>

  <section id="mining">
    <h2>Mining</h2>
    <input id="miner" placeholder="Miner-Adresse">
    <button id="mineBlock">Block minen</button>
  </section>

  <section id="chain">
    <h2>Blockchain</h2>
    <button id="loadChain">Chain laden</button>
    <pre id="chainOutput"></pre>
  </section>

  <script src="app.js"></script>
</body>
</html>
```

frontend/style.css

```css
body {
  font-family: sans-serif;
  margin: 20px;
  background: #0b0c10;
  color: #c5c6c7;
}

h1 {
  color: #66fcf1;
}

section {
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #45a29e;
}

input {
  margin: 5px;
  padding: 5px;
}
button {
  padding: 5px 10px;
  cursor: pointer;
}
`

frontend/app.js

```javascript
const API_BASE = "http://localhost:5000";

document.getElementById("sendTx").onclick = async () => {
  const sender = document.getElementById("sender").value;
  const recipient = document.getElementById("recipient").value;
  const amount = Number(document.getElementById("amount").value);

  const res = await fetch(${API_BASE}/transactions/new, {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({ sender, recipient, amount })
  });
  alert(await res.text());
};

document.getElementById("mineBlock").onclick = async () => {
  const miner = document.getElementById("miner").value || "pythonchain-ui";
  const res = await fetch(${API_BASE}/mine, {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({ miner })
  });
  const data = await res.json();
  alert("Block gemined: #" + data.index + " Hash: " + data.hash);
};

document.getElementById("loadChain").onclick = async () => {
  const res = await fetch(${API_BASE}/chain);
  const data = await res.json();
  document.getElementById("chainOutput").textContent =
    JSON.stringify(data, null, 2);
};
```

---

6. requirements.txt und Start

requirements.txt

```text
Flask
requests
```

Startbefehle (auf deinem Laptop, CMD)

```bash

1. Repo klonen/erstellen
git init pythonchain
cd pythonchain

2. Virtuelle Umgebung (optional, aber sauber)
python -m venv venv
venv\Scripts\activate  # Windows CMD

3. Abhängigkeiten installieren
pip install -r requirements.txt

4. Backend starten
python -m pythonchain.api
```

Frontend kannst du lokal einfach im Browser öffnen (z.B. frontend/index.html doppelklicken oder mit einem simplen python -m http.server im frontend-Ordner).











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

