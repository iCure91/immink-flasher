​🖼️ Immink Core
​Immink ist eine leichtgewichtige, Docker-basierte Steuerzentrale für smarte E-Paper Bilderrahmen.
Das System verbindet sich mit deinem lokalen Immich-Server, bereitet Bilder perfekt für E-Ink Displays auf und nutzt lokale KI-Modelle via Ollama, um smarte Bildtitel zu generieren.

​✨ Features
​🔌 Nahtlose Immich Integration: Zieht Bilder automatisch aus ausgewählten Alben (Standard & Priorität).

​🧠 KI-generierte Bildtitel: Nutzt lokale Vision-Modelle (wie llava:13b via Ollama), um kreative Bildunterschriften direkt auf dem Bild zu platzieren.

​📅 On-This-Day Memory: Erkennt historische Bilder zum aktuellen Datum und stellt sicher, dass kein Bild zweimal am selben Tag gezeigt wird.

​🎨 Perfektes E-Paper Rendering: Skaliert Bilder, fügt Ränder hinzu und wendet Floyd-Steinberg-Dithering an (unterstützt Schwarz/Weiß und Mehrfarb-Displays).

​🔒 Sichere Architektur: Strikt getrennte Ordnerstrukturen für Web-Code und private API-Keys.

​🐳 Unraid & Docker Ready: Kinderleicht über Docker Compose oder als Unraid-Template zu installieren.

​🛠️ Installation (Docker Compose)
​Das Projekt ist darauf ausgelegt, schnell und sicher per Docker gestartet zu werden.

1. ​Erstelle einen Projektordner (z.B./opt/immink oder /mnt/user/appdata/immink).
   
2. Erstelle darin eine docker-compose.yml mit folgendem Inhalt:

version: '3.8'

services:
  immink-core:
    image: icure91/immink:latest
    container_name: immink-core
    restart: unless-stopped
    ports:
      - "8040:80"    # Hier den gewünschten Port anpassen
    volumes:
      - ./data:/config/data   # Sicherer lokaler Speicher für Configs und Logs
    environment:
      - TZ=Europe/Berlin
    command: bash -c "chown -R www-data:www-data /config/www /config/data && apache2-foreground"

3. Starte den Container mit:
docker compose up -d

🚀 Erste Schritte & Konfiguration
​Sobald der Container läuft, wird beim ersten Aufruf automatisch der Setup-Wizard gestartet, da der data-Ordner noch leer ist.
​Öffne deinen Browser und rufe das Dashboard auf: http://<deine-server-ip>:8040
​Trage im Setup-Wizard deine Immich URL, den API Key und die Album-IDs ein.
​(Optional) Trage deine Ollama URL ein, wenn du KI-Bildtitel nutzen möchtest.
​Klicke auf Speichern. Das System generiert nun automatisch die sichere config.json im isolierten ./data Verzeichnis.

​📱 Hardware & ESP32 Flasher
​Immink bereitet die .bin und .jpg Dateien vor, die dann von einem ESP32-Microcontroller mit angeschlossenem E-Paper Display abgerufen werden.
​Um die Firmware auf deinen Bilderrahmen zu installieren, benötigst du keine zusätzliche Software. Verbinde den ESP32 einfach per USB mit deinem Computer und nutze den
browserbasierten Web-Installer:
​👉 https://icure91.github.io/immink-flasher/
(Hinweis: Benötigt Google Chrome oder Microsoft Edge)

​📂 Ordnerstruktur & Sicherheit
​Dieses Image wurde nach Best Practices für Sicherheit entwickelt.
​/config/www/: Beinhaltet die Logik und die generierten Bilder für den ESP32. Dieser Ordner ist nach außen sichtbar.
​/config/data/: Beinhaltet deine config.json (mit sensiblen API-Keys) und Laufzeit-Daten. Dieser Ordner liegt außerhalb des Web-Roots und kann nicht aus dem Netzwerk ausgelesen werden.

​🤝 Mitwirken
​Feedback, Bug-Reports und Pull Requests sind jederzeit willkommen!
