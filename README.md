WordPress-Installation mit MySQL, phpMyAdmin, Traefik (SSL) und Monitoring Stack.

## 🚀 Quick Start
```bash
# WordPress mit Traefik starten
docker-compose -f docker/docker-compose-traefik.yml up -d

# Monitoring starten
docker-compose -f docker/docker-compose-monitoring.yml up -d
```

## 📋 Services

### WordPress Stack
- **WordPress**: https://localhost
- **phpMyAdmin**: http://pma.localhost
- **Traefik Dashboard**: http://localhost:8080

### Monitoring Stack
- **Grafana**: http://localhost:3000 (admin/admin)
- **Prometheus**: http://localhost:9090
- **cAdvisor**: http://localhost:8888

## 🗂️ Struktur
```
wordpress-setup/
├── docker/
│   ├── docker-compose.yml          # Basic Setup
│   ├── docker-compose-traefik.yml  # Mit SSL
│   ├── docker-compose-monitoring.yml
│   └── prometheus.yml
└── ansible/
    └── backup-playbook.yml
```

## 🔧 Backup
```bash
ansible-playbook ansible/backup-playbook.yml
```

## 📊 Grafana Dashboards

1. Add Data Source → Prometheus → `http://prometheus:9090`
2. Import Dashboard → ID: `1860` oder `193`
