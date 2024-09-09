# Proxmox Monitoring with InfluxDB and Grafana

This project sets up a monitoring solution for Proxmox using InfluxDB for data storage and Grafana for visualization. The setup uses Docker containers for easy deployment and management.

![Alt text](/screenshots/dashboard_overview_grafana.png)

## Prerequisites

- Docker and Docker Compose installed on your system
- A running Proxmox server
- Basic understanding of Docker, InfluxDB, and Grafana

## Project Structure

```
Proxmox-monitoring-InfluxDB-Grafana/
│
├── docker
    ├── docker-compose.yml
├── screenshots
    ├── guide.md
└── README.md
```

## Setup Instructions

### 1. Docker Compose Configuration

Create a `docker-compose.yml` file with the following content:

```yaml

services:
  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: always
    ports:
      - 3000:3000
    networks:
      - monitor
    volumes:
      - "grafana-volume:/var/lib/grafana"
    user: "1000" # user pid: use id -u to find it, and you can add it like this or via env variables

  influxdb:
    image: influxdb:latest
    container_name: influxdb
    restart: always
    ports:
      - 8086:8086
      - 8089:8089/udp
    networks:
      - monitor
    volumes:
      - "influxdb-volume:/var/lib/influxdb"

networks:
  monitor:
    external: true

volumes:
  grafana-volume:
    external: true
  influxdb-volume:
    external: true
```

This Docker Compose file sets up two services:

1. **Grafana**: Running on port 3000, with a volume for persistent storage.
2. **InfluxDB**: Running on ports 8086 (HTTP) and 8089 (UDP), with a volume for persistent storage.

Both services are connected to a `monitor` network and use external volumes for data persistence.

### 2. Creating Docker Volumes and Network

Before starting the services, you need to create the Docker volumes and network referenced in the `docker-compose.yml` file. Run the following commands:

```bash
# Create Docker volumes
docker volume create influxdb-volume
docker volume create grafana-volume

# Create Docker network
docker network create monitor
```

These commands create the necessary volumes for data persistence and a shared network for communication between the containers.

### 3. Starting the Services

After creating the volumes and network, you can start the services by running:

```bash
docker-compose up -d
```

This command will bring both containers up in detached mode.

### 4. Configuring InfluxDB

1. Access InfluxDB at `http://<your-host>:8086`.
2. Set up an organization (e.g., "HomeServer") and a bucket (e.g., "proxmox").
3. Create an API token:
   - Go to "Load Data" > "API Tokens".
   - Copy the token associated with your user.

### 5. Setting up InfluxDB in Proxmox

1. In Proxmox, go to Datacenter > Metric Server.
2. Click "Add" and select "InfluxDB".
3. Fill in the details:
   - Name: Choose a name
   - Server: Your InfluxDB host IP or hostname
   - Port: 8086
   - Protocol: HTTP
   - Organization: Your InfluxDB organization name
   - Bucket: Your InfluxDB bucket name
   - Token: Paste your API token

### 6. Configuring Grafana

1. Access Grafana at `http://<your-host>:3000`.
2. Log in with default credentials (admin/admin) and set a new password.
3. Add InfluxDB as a data source:
   - Go to Configuration > Data Sources > Add data source.
   - Select InfluxDB.
   - Change Query Language to "Flux".
   - Set the URL to your InfluxDB host and port.
   - Disable Basic Auth and enable Skip TLS Verify.
   - Fill in the InfluxDB Details with your organization, token, and default bucket.
   - Click "Save & Test".

### 7. Importing a Grafana Dashboard

1. Go to [Proxmox Cluster [Flux] Grafana Dashboard (ID 15356)](https://grafana.com/grafana/dashboards/15356).
2. Copy the dashboard ID.
3. In Grafana, click the "+" icon and select "Import".
4. Paste the dashboard ID and click "Load".
5. Select your InfluxDB data source and click "Import".

## Verification

To confirm data is flowing:

1. In InfluxDB, go to "Data Explorer".
2. Select your bucket and check for data points.

## Troubleshooting

- Ensure all services are running: `docker-compose ps`
- Check service logs: `docker-compose logs grafana` or `docker-compose logs influxdb`
- Refer to [the Screenshots](./screenshots/guide.md) in case something is not clear

## Contributing

Feel free to open issues or submit pull requests for improvements or bug fixes.
