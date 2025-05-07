# Prometheus and Grafana Advanced Monitoring

This project sets up a complete monitoring solution using **Ansible**, featuring:

- ✅ **Prometheus** (metrics collection)
- ✅ **Node Exporter** (system-level metrics)
- ✅ **Grafana** (visualization)
- ✅ **Alertmanager** (notification system)
- ✅ **Custom Alert Rules** (e.g., High CPU Usage)

---

## 📦 Project Structure

```
prometheus-grafana-advanced-monitoring/
├── inventory.ini
├── playbook.yml
└── roles/
    ├── prometheus/
    │   ├── tasks/main.yml
    │   └── templates/
    │       ├── prometheus.yml.j2
    │       ├── alert.rules.yml.j2
    │       └── alertmanager.yml.j2
    └── grafana/
        ├── tasks/
        │   ├── main.yml
        │   └── configure_datasource.yml
        └── files/
            └── node_exporter_dashboard.json
```

---

## 🚀 How to Use

1. **Edit `inventory.ini`** with your server IP and SSH user.
2. **Configure Alertmanager SMTP in `alertmanager.yml.j2`.**
3. Run the playbook:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

## 📊 Collected Metrics

With Node Exporter:
- **CPU Usage**: `rate(node_cpu_seconds_total{mode!="idle"}[1m]) * 100`
- **Memory Usage**: `node_memory_MemAvailable_bytes`
- **Disk Usage**: `node_filesystem_avail_bytes`
- **Network Stats**: `node_network_receive_bytes_total`

---

## 📈 Grafana

- Access Grafana at: `http://<your-ip>:3000`
- Login: `admin / admin` (default)
- Example dashboard: `Node Exporter Dashboard` (auto-loaded)

---

## 🔔 Alerts

- Defined in: `alert.rules.yml`
- Example Rule:

```yaml
- alert: HighCPUUsage
  expr: rate(node_cpu_seconds_total{mode!="idle"}[1m]) * 100 > 80
  for: 1m
  labels:
    severity: warning
  annotations:
    summary: "High CPU usage detected"
```

---

## ✉️ Alert Notifications

- Managed by **Alertmanager**
- Sent via **email**
- Edit SMTP details in `alertmanager.yml.j2`:

```yaml
email_configs:
  - to: 'admin@example.com'
    from: 'prometheus@example.com'
    smarthost: 'smtp.example.com:587'
    auth_username: 'prometheus@example.com'
    auth_password: 'your_password'
```

---

## 🧰 Requirements

- Ubuntu 20.04+
- Python, Ansible installed
- Internet access (to fetch Prometheus, Grafana, etc.)

---

## 📌 Notes

- You can extend the alerts or add Slack/Telegram later
- Grafana dashboards can be exported/imported via JSON

---


