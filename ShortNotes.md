# 🐳 Docker & Docker Compose – Prüfungsnotizen

## 📄 Dockerfile – Grundstruktur & Befehle

```Dockerfile
FROM python:3.10-slim        # Basis-Image
WORKDIR /app                 # Arbeitsverzeichnis im Container
COPY . .                     # Kopiert lokalen Inhalt ins Image
RUN pip install flask        # Führt Befehl beim Bauen aus
CMD ["python", "app.py"]     # Startbefehl beim Containerstart
```

**Weitere wichtige Befehle:**
- `ENV NAME=value` – Umgebungsvariablen setzen
- `EXPOSE 5000` – Informativ: Port, der verwendet wird
- `ENTRYPOINT ["..."]` – Alternativer Startbefehl

---

## 🧪 Beispiel-App zum Testen

**`app.py`:**
```python
from flask import Flask
import os
app = Flask(__name__)
@app.route('/')
def hello(): return f"Hello, {os.environ.get('SECRET_KEY')}"
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Build & Run:**
```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

---

## ⚙️ Docker Compose – YAML Beispiel

**`docker-compose.yml`:**
```yaml
version: "3.8"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    env_file:
      - .env
```

**`.env`:**
```
SECRET_KEY=supersecret
```

**Starten:**
```bash
docker-compose up --build
```

---

## 🔐 Umgang mit Secrets in Compose

**Drei gängige Wege:**
1. `.env`-Datei → automatisch oder via `env_file:` eingebunden
2. Direkt im `environment:` Block (nicht empfohlen für Secrets)
3. `secrets:` (ab Docker Swarm)

```yaml
services:
  web:
    environment:
      - SECRET_KEY=${SECRET_KEY}
```

---

## 🛠️ Nützliche CLI-Kommandos

```bash
docker build -t name .              # Image bauen
docker run -p 5000:5000 name        # Container starten
docker-compose up                   # Compose starten
docker-compose logs web             # Logs ansehen
docker-compose ps                   # Laufende Services
docker exec -it <container> sh      # In Container reinschauen
```

---

## 📘 Minimal-Dokumentation (README.md Style)

```markdown
# Docker Demo App

## Starten
```bash
docker-compose up --build
```

## Zugriff
- http://localhost:5000

## Umgebungsvariablen
- SECRET_KEY → wird via `.env` geladen
```
