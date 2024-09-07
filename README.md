# Network Monitoring Project

## Overview

This project sets up a comprehensive network monitoring solution using pfSense, custom Python scripts, Telegraf, InfluxDB, Prometheus, and Grafana. It allows you to collect, store, and visualize network metrics from a pfSense firewall, providing insights into bandwidth consumption, upload and download rates, and other network activities.

## Components

1. **pfSense**: Firewall and network management.
2. **Custom Python Script**: Collects network metrics from pfSense.
3. **Telegraf**: Collects and forwards data from the Python script.
4. **InfluxDB**: Time-series database for storing metrics.
5. **Prometheus**: Metrics collection and storage system.
6. **Grafana**: Data visualization and dashboard creation.

## Prerequisites

- pfSense installed and configured with LAN and DHCP setup
- VMs deployed as clients within the LAN
- Docker and Docker Compose installed on a management machine
- Access to pfSense shell for script deployment and Telegraf installation

## Project Structure

```
pfsense-monitoring/
├── pfsense/
│   ├── config/
│   │   └── firewall_rules.txt
│   └── scripts/
│       └── network_metrics.py
├── telegraf/
│   └── telegraf.conf
├── docker/
│   └── docker-compose.yml
└── README.md
```

## Setup Instructions

1. **Clone the Repository**
   ```
   git clone https://github.com/yourusername/network-monitoring-project.git
   cd network-monitoring-project
   ```

2. **pfSense Configuration**
   - Copy `pfsense/scripts/network_metrics.py` to your pfSense machine.
   - Configure firewall rules as described in `pfsense/config/firewall_rules.txt`.
   - Install Telegraf on pfSense and copy `telegraf/telegraf.conf` to the appropriate location.

3. **Docker Setup**
   - Navigate to the `docker` directory.
   - Run `docker-compose up -d` to start InfluxDB, Prometheus, and Grafana.

4. **Grafana Configuration**
   - Access Grafana at `http://management-machine-ip:3000`.
   - Add InfluxDB and Prometheus as data sources.

5. **Prometheus Configuration**
   - Ensure `prometheus.yml` is correctly set up to scrape pfSense metrics.

6. **InfluxDB Configuration**
   - Access InfluxDB at `http://management-machine-ip:8086`.
   - Set up an organization and bucket for the pfSense metrics.
   - Configure Telegraf to write to this bucket.

## Usage

Once set up, you can access the Grafana dashboard to monitor your network metrics:

1. Open a web browser and navigate to `http://management-machine-ip:3000`.
2. Log in to Grafana (default credentials are admin/admin).
3. Select the "PfSense Network Monitoring" dashboard.
4. View real-time and historical data on bandwidth consumption, upload and download rates, and other network metrics.

## Customization

- Modify `network_metrics.py` to collect additional metrics as needed.
- Adjust the Grafana dashboard (`network_monitoring.json`) to display metrics relevant to your needs.
- Update `telegraf.conf` to change data collection intervals or add new inputs/outputs.

## Troubleshooting

- Ensure all firewall rules are correctly set up to allow traffic between components.
- Check Telegraf logs on pfSense for any data collection or forwarding issues.
- Verify that Prometheus is successfully scraping metrics from pfSense.
- Review InfluxDB logs to ensure data is being received and stored correctly.

## Contributing

Contributions to this project are welcome! Please fork the repository and submit a pull request with your changes.

## Acknowledgments

- pfSense community for their extensive documentation and support.
- InfluxData, Prometheus, and Grafana teams for their excellent open-source tools.
