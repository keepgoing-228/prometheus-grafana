# prometheus-grafana

This repository contains a complete monitoring stack using Prometheus, Grafana, Node Exporter, and Alertmanager, all configured with Docker Compose.

## Services Included

- **Prometheus** (Port 9090): Metrics collection and storage
- **Grafana** (Port 3000): Visualization and dashboards
- **Node Exporter** (Port 9100): System metrics collection
- **Alertmanager** (Port 9093): Alert handling and routing

## Quick Start

### Prerequisites

- Docker and Docker Compose (v2+) installed

### Setup Instructions

1. **Clone and navigate to the repository:**
   ```bash
   git clone https://github.com/keepgoing-228/prometheus-grafana.git
   cd prometheus-grafana
   ```

2. **Start the monitoring stack:**
   ```bash
   docker compose up -d
   ```

3. **Verify all services are running:**
   ```bash
   docker compose ps
   curl http://localhost:9090/-/ready
   curl -u admin:admin http://localhost:3000/api/datasources
   ```

Prometheus and Grafana data are stored in named Docker volumes (`prometheus_data`, `grafana_data`), so no manual directory creation or `chown` is required.

## Access the Services

- **Grafana**: http://localhost:3000
  - Username: `admin`
  - Password: `admin`
  - A **Prometheus** datasource (`http://prometheus:9090`) is provisioned automatically.
- **Prometheus**: http://localhost:9090
- **Alertmanager**: http://localhost:9093
- **Node Exporter**: http://localhost:9100

## Configuration Files

- `docker-compose.yaml`: Service definitions and volumes
- `prometheus.yml`: Prometheus configuration (scrapes itself and Node Exporter)
- `alert_rules.yml`: Alert rules for Prometheus
- `alertmanager.yml`: Alertmanager configuration (Slack receiver)
- `grafana/provisioning/datasources/prometheus.yml`: Grafana datasource provisioning
- `grafana/provisioning/dashboards/dashboards.yml`: Grafana dashboard provider — drop any dashboard JSON into `grafana/provisioning/dashboards/` and it is loaded automatically (e.g. export [Node Exporter Full, ID 1860](https://grafana.com/grafana/dashboards/1860))

## Operations

### Stopping the Stack

```bash
docker compose down          # keep data volumes
docker compose down -v       # also delete Prometheus/Grafana data
```

### Viewing Logs

```bash
docker compose logs -f
docker compose logs grafana
docker compose logs prometheus
```

### Reloading configuration

```bash
# After editing prometheus.yml / alert_rules.yml
docker compose restart prometheus
# After editing alertmanager.yml
docker compose restart prometheus-alert
```

## Troubleshooting

- **Grafana shows no data**: check `docker compose logs grafana` for provisioning errors and confirm the datasource health at
  Connections → Data sources → Prometheus → *Save & test*.
- **Target down in Prometheus**: open http://localhost:9090/targets; all services share the compose network and are addressed by service name (`prometheus`, `node-exporter`, `prometheus-alert`).
- **Alert never fires**: `HighAverageCPUTemperature` relies on `node_hwmon_temp_celsius`, which requires host hardware sensors (often absent in VMs).
