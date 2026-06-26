# Vehicle telemetry deployment

This standalone package runs Node-RED, InfluxDB, and Grafana without requiring
the ESP32 firmware source repository.

The package includes the provisioned Node-RED flow and Grafana dashboard used by
the vehicle telemetry stack, including the DTC monitor and Clear DTC control.

## Requirements

- Docker Desktop with Docker Compose
- the ESP32 and Docker host connected to reachable networks

## First run

Open Command Prompt in this directory and create the local environment file:

```cmd
copy .env.example .env
notepad .env
```

Replace every `CHANGE_ME` value. The InfluxDB password must contain at least
eight characters. Initially set `GRAFANA_INFLUXDB_TOKEN` to the same value as
`INFLUXDB_TOKEN`, and use a different random value for
`NODE_RED_CREDENTIAL_SECRET`.

Set `ESP32_WS_URL` to the ESP32 WebSocket address reachable from this computer,
then start the stack:

```cmd
docker compose pull
docker compose up -d
docker compose ps
```

After the services become healthy, open:

- Node-RED: <http://localhost:1880>
- InfluxDB: <http://localhost:8086>
- Grafana: <http://localhost:3000>

For another device on the same LAN, replace `localhost` with the Docker host's
IPv4 address and allow the required ports through its firewall.

## Operations

```cmd
docker compose logs -f
docker compose restart
docker compose down
```

Do not run `docker compose down -v` unless all stored telemetry and dashboards
may be deleted.

## Updating

When this deployment package is updated, replace the package files and run:

```cmd
docker compose pull
docker compose up -d
```

The Node-RED flow is mounted from `node-red/flows.json`, so package updates can
update the deployed flow even when the Docker volume already exists.
