# WordPress Setup - Docker + Ansible + Backups

Automatisiertes WordPress-Setup mit Docker, Ansible für Backups und tägliche Sicherung.

## Projektstruktur

```
wordpress-setup/
├── docker/
│   ├── docker-compose.yml      # WordPress + MySQL + phpMyAdmin
│   └── traefik/                # Reverse Proxy Config
├── ansible/
│   ├── backup-playbook.yml     # Tägliche DB + Datei Backups
│   └── inventory.ini           # Ansible Hosts
├── README.md                   # Diese Datei
└── .gitignore
```

## Quickstart

### 1. Repository clonen
```bash
git clone git@github.com:Holle030/wordpress-setup.git
cd wordpress-setup/docker
```

### 2. Container starten
```bash
docker-compose up -d
docker-compose ps
```

### 3. WordPress aufrufen
```
http://localhost:8080
```

- **WordPress:** http://localhost:8080
- **phpMyAdmin:** http://localhost:8081 (wpuser / wppass123)

## Docker-Compose Services

| Service | Port | Funktion |
|---------|------|----------|
| wordpress | 8080 | WordPress mit Apache |
| mysql_db | 3306 | MySQL 8.0 Datenbank |
| phpmyadmin | 8081 | Web-DB-Admin-Tool |

## Umgebungsvariablen

Konfiguriert in `docker-compose.yml`:

```yaml
WORDPRESS_DB_HOST: mysql_db
WORDPRESS_DB_USER: wpuser
WORDPRESS_DB_PASSWORD: wppass123
WORDPRESS_DB_NAME: wordpress
MYSQL_ROOT_PASSWORD: rootpass123
```

## Backups mit Ansible

### Backup-Struktur
- **Backup-Dir:** `/home/holle030/wordpress-backups`
- **Schedule:** Täglich um 2:00 Uhr (Cron)
- **Aufbewahrung:** Automatisch löschen nach 7 Tagen

### Backup jetzt ausführen
```bash
cd ~/wordpress-setup/ansible
ansible-playbook backup-playbook.yml
```

### Backups ansehen
```bash
ls -lh ~/wordpress-backups
```

Enthält:
- `wordpress-files-*.tar.gz` → WordPress-Dateien
- `wordpress-db-*.sql` → MySQL-Datenbank-Dump

### Cron-Job (automatisch täglich 2:00 Uhr)
```bash
crontab -l
```

Zeigt: `0 2 * * * cd ~/wordpress-setup/ansible && ansible-playbook backup-playbook.yml`

## Datenbank-Zugriff

### phpMyAdmin Web-Interface
```
http://localhost:8081
User: wpuser
Password: wppass123
```

### CLI Zugriff
```bash
docker exec mysql_db mysql -u wpuser -pwppass123 wordpress
```

## Container-Management

### Alle Container stoppen
```bash
cd docker
docker-compose down
```

### Alle Daten löschen (ACHTUNG!)
```bash
docker-compose down -v
```

### Logs anschauen
```bash
docker-compose logs -f wordpress
docker-compose logs -f mysql_db
```

## Volumes (Persistente Daten)

- `wordpress_data` → WordPress-Installation & Themes
- `db_data` → MySQL-Datenbank-Dateien

Automatisch bei `docker-compose up` erstellt.

## Prüfungs-Relevanz (AP2 Systemintegration)

Dieses Projekt demonstriert:

- **Docker:** Multi-Container Application
- **Ansible:** Automatisierung & Backup-Strategie
- **Backup-Konzept:** Täglich, automatisiert, zeitgesteuert
- **Monitoring:** Ansible Playbook als IaC (Infrastructure as Code)
- **Sicherheit:** Datenbank-Isolation, Password-Management

## Troubleshooting

### WordPress zeigt "Database Error"
```bash
# MySQL muss bereit sein - 10 Sekunden warten
sleep 10
curl http://localhost:8080
```

### Container starten nicht
```bash
docker-compose logs mysql_db
```

### Backup schlägt fehl
```bash
docker ps
# Container-Namen checken & backup-playbook.yml anpassen
```




## Quellen

- [Docker Docs](https://docs.docker.com)
- [Ansible Docs](https://docs.ansible.com)
- [WordPress Docker Image](https://hub.docker.com/_/wordpress)
- [MySQL Docker Image](https://hub.docker.com/_/mysql)

