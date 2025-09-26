# prometheus-grafana

This repository contains a complete monitoring stack using Prometheus, Grafana, Node Exporter, and Alertmanager, all configured with Docker Compose.

## Services Included

- **Prometheus** (Port 9090): Metrics collection and storage
- **Grafana** (Port 3000): Visualization and dashboards
- **Node Exporter** (Port 9100): System metrics collection
- **Alertmanager** (Port 9093): Alert handling and routing

## Quick Start

### Prerequisites

- Docker and Docker Compose installed
- Linux/macOS system (for proper file permissions)

### Setup Instructions

1. **Clone and navigate to the repository:**
   ```bash
   git clone https://github.com/keepgoing-228/prometheus-grafana.git
   cd prometheus-grafana
   ```

2. **Set up Grafana data directory permissions:**

   Grafana runs with UID/GID 472 inside the container, so we need to ensure the data directory has the correct permissions:
   ```bash
   # Create the grafana_data directory if it doesn't exist
   mkdir -p ./grafana_data

   # Set proper ownership for Grafana (UID/GID 472)
   sudo chown -R 472:472 ./grafana_data

   # Set proper permissions
   sudo chmod -R 755 ./grafana_data
   ```

3. **Start the monitoring stack:**
   ```bash
   docker compose up -d
   ```

4. **Verify all services are running:**
   ```bash
   docker compose ps
   ```

## Access the Services

- **Grafana**: http://localhost:3000
  - Username: `admin`
  - Password: `admin`
- **Prometheus**: http://localhost:9090
- **Alertmanager**: http://localhost:9093
- **Node Exporter**: http://localhost:9100

## Configuration Files

- `prometheus.yml`: Prometheus configuration
- `alert_rules.yml`: Alert rules for Prometheus
- `alertmanager.yml`: Alertmanager configuration
- `grafana/`: Grafana provisioning directory

## Important Notes

### Grafana Data Persistence

The `./grafana_data` directory must be writable by UID/GID 472 (Grafana's user inside the container). If you encounter permission issues:

```bash
# Check current ownership
ls -la ./grafana_data

# Fix ownership if needed
sudo chown -R 472:472 ./grafana_data
```

### Stopping the Stack

```bash
docker compose down
```

### Viewing Logs

```bash
# View all logs
docker compose logs

# View logs for specific service
docker compose logs grafana
docker compose logs prometheus
```

## Troubleshooting

If Grafana fails to start due to permission issues:

1. Check the ownership of `./grafana_data`:
   ```bash
   ls -la ./grafana_data
   ```

2. Fix ownership:
   ```bash
   sudo chown -R 472:472 ./grafana_data
   ```

3. Restart the services:
   ```bash
   docker compose restart grafana
   ```
