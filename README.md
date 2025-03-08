# Nginx Proxy Manager mit Docker Compose

Dieses Repository enthält die notwendigen Konfigurationsdateien, um Nginx Proxy Manager mit Docker Compose auf einfache Weise einzurichten. Diese Lösung ermöglicht es Ihnen, eine leistungsstarke Nginx-basierte Reverse-Proxy-Lösung mit einer benutzerfreundlichen Weboberfläche zu betreiben.

## 📋 Übersicht

Nginx Proxy Manager bietet eine grafische Benutzeroberfläche zum Verwalten von Nginx-Proxy-Hosts mit Features wie:

- SSL-Unterstützung mit automatischer Let's Encrypt-Integration
- Weiterleitung von Anfragen an verschiedene lokale oder entfernte Dienste
- Zugriffsschutz mit einfacher Authentifizierung
- Verwaltung von benutzerdefinierten SSL-Zertifikaten
- Umfangreiche Protokollierungsfunktionen

Die Konfiguration nutzt Docker Compose mit MariaDB als Datenbank-Backend und ermöglicht eine einfache Anpassung über Umgebungsvariablen.

## 🔧 Voraussetzungen

- Docker (empfohlen: Version 20.10.0 oder höher)
- Docker Compose (empfohlen: Version 2.0.0 oder höher)
- Ein System mit offenen Ports 80, 443 und 81 (oder angepasst in der .env-Datei)

## 🚀 Installation

1. Klonen Sie dieses Repository:
   ```bash
   git clone https://github.com/ihr-username/nginx-proxy-manager.git
   cd nginx-proxy-manager
   ```

2. Passen Sie bei Bedarf die Umgebungsvariablen in der `.env`-Datei an (siehe Konfiguration).

3. Starten Sie die Container:
   ```bash
   docker-compose up -d
   ```

4. Greifen Sie auf die Admin-Oberfläche zu unter:
   ```
   http://ihre-server-ip:81
   ```

5. Melden Sie sich mit den Standardanmeldedaten an:
   - E-Mail: `admin@example.com`
   - Passwort: `changeme`

6. **Wichtig**: Ändern Sie sofort das Standardpasswort nach der ersten Anmeldung!

## ⚙️ Konfiguration

Die Konfiguration erfolgt über die `.env`-Datei. Hier sind die verfügbaren Optionen:

| Variable | Beschreibung | Standardwert |
|----------|-------------|--------------|
| `NGINX_IMAGE` | Docker Image für Nginx Proxy Manager | `jc21/nginx-proxy-manager:latest` |
| `HTTP_PORT` | HTTP Port (Host) | `80` |
| `HTTPS_PORT` | HTTPS Port (Host) | `443` |
| `ADMIN_PORT` | Admin-Web-Interface Port (Host) | `81` |
| `DB_MYSQL_HOST` | MariaDB Hostname | `db` |
| `DB_MYSQL_PORT` | MariaDB Port | `3306` |
| `DB_MYSQL_USER` | MariaDB Benutzername | `npm` |
| `DB_MYSQL_PASSWORD` | MariaDB Passwort | `npm` |
| `DB_MYSQL_NAME` | MariaDB Datenbankname | `npm` |
| `DISABLE_IPV6` | IPv6-Unterstützung deaktivieren | `true` |
| `DB_IMAGE` | Docker Image für MariaDB | `jc21/mariadb-aria:latest` |
| `MYSQL_ROOT_PASSWORD` | MariaDB Root-Passwort | `npm` |
| `MYSQL_DATABASE` | Zu erstellende Datenbank | `npm` |
| `MYSQL_USER` | Zu erstellender Datenbankbenutzer | `npm` |
| `MYSQL_PASSWORD` | Passwort für Datenbankbenutzer | `npm` |
| `MARIADB_AUTO_UPGRADE` | Automatisches Upgrade der Datenbank | `1` |

**Hinweis zur Sicherheit**: Für Produktionsumgebungen sollten Sie unbedingt die Standard-Passwörter in der `.env`-Datei ändern!

## 📁 Dateien im Repository

- `docker-compose.yml` - Die Hauptkonfigurationsdatei für Docker Compose
- `.env` - Umgebungsvariablen für die Konfiguration
- `README.md` - Diese Dokumentation

## 💾 Persistente Daten

Die folgenden Docker Volumes werden für persistente Daten verwendet:

- `data` - Konfigurationsdaten des Nginx Proxy Managers
- `letsencrypt` - Let's Encrypt Zertifikate und Konfiguration
- `mysql` - MariaDB Datenbankdaten

## 🔍 Troubleshooting

### Die Weboberfläche ist nicht erreichbar

1. Überprüfen Sie, ob die Container laufen:
   ```bash
   docker-compose ps
   ```

2. Prüfen Sie die Logs auf Fehler:
   ```bash
   docker-compose logs app
   docker-compose logs db
   ```

3. Stellen Sie sicher, dass die Ports nicht von anderen Diensten belegt sind:
   ```bash
   netstat -tulpn | grep -E '80|81|443'
   ```

### Datenbank-Verbindungsfehler

Wenn der Nginx Proxy Manager keine Verbindung zur Datenbank herstellen kann:

1. Stellen Sie sicher, dass der MariaDB-Container läuft:
   ```bash
   docker-compose logs db
   ```

2. Überprüfen Sie die Zugangsdaten in der `.env`-Datei

3. Bei Bedarf können Sie die Datenbank zurücksetzen:
   ```bash
   docker-compose down -v
   docker-compose up -d
   ```
   **Achtung**: Dies löscht alle konfigurierten Proxy-Hosts und Einstellungen!

## 📋 Updates

Um auf eine neuere Version zu aktualisieren:

```bash
# Repository aktualisieren (falls nötig)
git pull

# Container mit neuen Images starten
docker-compose pull
docker-compose up -d
```

## 📜 Lizenz

Dieses Projekt steht unter der MIT-Lizenz. Siehe die LICENSE-Datei für Details.

## 🔗 Nützliche Links

- [Offizielle Nginx Proxy Manager Dokumentation](https://nginxproxymanager.com/guide/)
- [Docker-Dokumentation](https://docs.docker.com/)
- [Docker Compose-Dokumentation](https://docs.docker.com/compose/)

---

Bei Fragen oder Problemen eröffnen Sie bitte ein Issue in diesem Repository.
