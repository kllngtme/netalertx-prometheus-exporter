<a href="https://www.buymeacoffee.com/kllngtme" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 30px !important;width: 108px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a><br>

DEPRECATED - this feature is now native to NetAlertX<br>
https://github.com/jokob-sk/NetAlertX/releases/tag/v25.8.6<br>
https://jokob-sk.github.io/NetAlertX/API/?h=prom

## NetAlertX Prometheus Exporter for Grafana
This is a way to import data from NetAlertX to Grafana graphs.
A simple Flask-based Prometheus exporter for NetAlertX device statistics.

NetAlertX > netalertx_exporter > Prometheus > Grafana


<img width="1886" height="1338" alt="471045109-98c2d49e-a9ae-4de0-b6f4-4a06e034e840" src="https://github.com/user-attachments/assets/94d37020-00d6-46ef-831b-dc0a57b0086c" />

This exposes the following items for use in Grafana:
```
netalertx_total_devices
netalertx_connected_devices
netalertx_down_devices
netalertx_favorite_devices
netalertx_new_devices
netalertx_archived_devices
```

Per Device statistics it can pull as well:
```
Device
Device Type
Devices Status: (Online, Offline, or Down)
MAC
IP
Vendor
First Connect/Last Connect
```

## REQUIREMENTS
- NetAlertX | https://github.com/jokob-sk/NetAlertX/
- Grafana with Prometheus | https://grafana.com/docs/grafana/latest/getting-started/get-started-grafana-prometheus/

##  Instructions
Update all the specifics for your own server details. Run this docker image, update your prometheus.yml with your netalertx_exporter info. That should be it for the "bridge".
I would just do a quick check with popping this in your browser and see if you have results: http://:netalertx-exporterIP:9000/metrics
You should be good to use the data in Grafana at that point. You can use my Grafana Dashboard or build your own of course.

```
docker run -d \
  --name netalertx_exporter \
  -e NETALERTX_IP=192.168.1.35 \
  -e NETALERTX_PORT=20211 \
  -e API_KEY=your_api_key_here \
  -p 9000:9000 \
  kllngtme/netalertx_exporter:latest
```

Docker-Compose.yml:
```
services:
  netalertx-exporter:
    image:  kllngtme/netalertx_exporter:latest
    container_name: netalertx_exporter
    ports:
      - "9000:9000"
    environment:
      NETALERTX_IP: 192.168.1.35     # Replace with your NetAlertX IP
      NETALERTX_PORT: 20211          # Default NetAlertX port
      API_KEY: your_api_key_here     # Replace with your NetAlertX API key
    restart: unless-stopped
```

Prometheus.yml:
```
  - job_name: 'netalertx'
    scrape_interval: 60s
    static_configs:
      - targets: ['netalertx_exporterIP:9000']
```

## Environment Variables

| Variable     | Description                            | Required | Default |
|--------------|------------------------------------|----------|---------|
| `NETALERTX_IP`  | IP or hostname of your NetAlertX server | Yes      | —       |
| `NETALERTX_PORT`| Port number for NetAlertX API         | No       | 20211   |
| `API_KEY`       | API key for accessing NetAlertX API    | Yes      | —       |

----------------------------------

Access and test netalertx-exporter metrics with:

If within container: ```curl http://localhost:9000/metrics```

Outside of host: ```curl http://:netalertx-exporterIP:9000/metrics```



Testing along the way if needed. You should be able to use this to test directly with your NetAlertX server:

```curl -H "X-API-Key: t_ZzKDKaYmBWcJ96bi63o6" http://192.168.1.35:20211/php/server/devices.php?action=getDevices```

```curl -H "X-API-Key: t_ZzKDKaYmBWcJ96bi63o6" http://192.168.1.35:20211/php/server/devices.php?action=getDevicesTotals```

