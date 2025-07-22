# NetAlertX Prometheus Exporter for Grafana
This is a way to import data from NetAlertX to Grafana graphs.
A simple Flask-based Prometheus exporter for NetAlertX device statistics.

## REQUIREMENTS
- NetAlertX
- Grafana with Prometheus

## Features
- Exposes device totals, connected, down, and new devices as Prometheus metrics.
- Supports environment variables for easy configuration.
- Compatible with Docker for quick deployment.

## Environment Variables

| Variable     | Description                            | Required | Default |
|--------------|------------------------------------|----------|---------|
| `NETALERTX_IP`  | IP or hostname of your NetAlertX server | Yes      | —       |
| `NETALERTX_PORT`| Port number for NetAlertX API         | No       | 20211   |
| `API_KEY`       | API key for accessing NetAlertX API    | Yes      | —       |


Docker-Compose example:
```
services:
  netalertx-exporter:
    container_name: netalertx-exporter
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "9000:9000"
    environment:
      NETALERTX_IP: 192.168.1.35
      NETALERTX_PORT: 20211
      API_KEY: t_ZzKDKaYmBWcJ96bi63o6
    restart: unless-stopped
```

Access and test netalertx-exporter metrics with:

If within container: ```curl http://localhost:9000/metrics```

Outside of host: ```curl http://:netalertx-exporterIP:9000/metrics```



Testing along the way if needed. You should be able to use this to test directly with your NetAlertX server:

```curl -H "X-API-Key: t_ZzKDKaYmBWcJ96bi63o6" http://192.168.1.35:20211/php/server/devices.php?action=getDevices```

```curl -H "X-API-Key: t_ZzKDKaYmBWcJ96bi63o6" http://192.168.1.35:20211/php/server/devices.php?action=getDevicesTotals```


Prometheus.yml:
```
  - job_name: 'netalertx'
    static_configs:
      - targets: ['*NETALERTX-EXPORTERIP*:9000']
```
<img width="1616" height="633" alt="screenshot" src="https://github.com/user-attachments/assets/0a2e0026-d815-4c5f-af05-abc6692bec55" />
