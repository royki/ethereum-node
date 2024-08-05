# Ethereum RPC node

- Simple Docker Compose file to run an Ethereum RPC node using the Geth execution client and Prysm consensus client.
- It also includes a monitoring and alerting stack using Grafana, Prometheus, Alertmanager, Cadvisor, Node Exporter, Eth Metrics Exporter, Prommail, and Loki.
- Before executing the Docker Compose command, the user needs to generate a JWT secret and save it in a `.jwt.hex` file in the same location as the `docker-compose.yaml` file.
- Considering Eth node and monitoring are in same server

## Clone the repository

- `git clone https://github.com/royki/ethereum-node.git`
- `cd ~/ethereum-node/infra/docker-setup`

## Command to Generate and Save JWT Secret

- `openssl rand -hex 32 | tr -d "\n" > .jwt.hex`

### For monitoring, it has provided monitoring, grafana, prometheus, loki, aletmanager directories with config and dashboard files

- Before executing docker compose command, check all directories are in right place
- Update `slack_api_url` and `channel` name value in `~/ethereum-node/infra/docker-setup/monitoring/alertmanager/alertmanager.yaml`

### Run docker compose to run and up Eth RPC services and Monitoring services

- Assuming Docker and Docker Compose are already installed on the server and the system meets the requirements for an RPC node.
- `docker compose up -d`
- `docker compose ps`

### Monitoring URLs

- Gragana: `http://Instance_IP:3000`
  - Grafana `login` is disabled
  - List of dashboards
    - `Docker Container & Host Metrics`
    - `Execution client & Consensus client logs`
    - `Ethereum Metrics Exporter (Single)`
    - `Geth Dashboard`
    - `Prysm Dashboard`

- Prometheus: `http://Instance_IP:9090`
- Alerts - `http://Instance_IP/alerts`
  - List of alerts - `InstanceDown`, `HighCPUUsage`, `HighMemoryUsage`, `HighDiskUsage`, `NumberOfPeersLow (Execution Client)`, `NumberOfPeersLow (Consensus Client)`
- Alert rules - `http://Insance_IP:9090/rules`
  - Check [alert_rules.yaml](https://github.com/royki/ethereum-node/blob/master/infra/docker-setup/monitoring/prometheus/alert_rules.yaml) file for alert rules details.
- Prometheus metrics target - `http://Instance_IP:9090/targets`
