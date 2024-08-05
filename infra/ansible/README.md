# Infra configuration

- Ansible roles
  - [ethnode](https://github.com/royki/ethereum-node/tree/master/infra/ansible/roles/ethnode) [Geth client, Prysm client with Cadvisor, Node Exporter]]
  - [monitoring](https://github.com/royki/ethereum-node/tree/master/infra/ansible/roles/monitoring) [Grafana, Prometheus, Alertmanager, Eth Metrics Exporter, Loki configuration]

## Run Ansible from local

- For `prysm` client [prysm_suggested_fee_recipient](https://github.com/royki/ethereum-node/blob/master/infra/ansible/roles/ethnode/defaults/main.yaml#L66) address. [Optional]

- For monitoring
  - Update [monitoring_slack_api_url](<https://github.com/royki/ethereum-node/tree/master/infra/ansible/roles/monitoring/defaults/main.yaml#L67>
  - Update [monitoring_slack_channel](<https://github.com/royki/ethereum-node/tree/master/infra/ansible/roles/monitoring/defaults/main.yaml#L68>

- Example of `host.cfg`

    ```shell
    [eth-node]
    eth-rpc-1 ansible_host=X.X.X.X

    [monitor]
    monitoring-1 ansible_host=Y.Y.Y.Y

    [all:vars]
    ansible_ssh_port=22
    ansible_connection=ssh
    ansible_ssh_user=ubuntu
    ansible_ssh_private_key_file=UPDATE_SSH_FILE_PATH
    ansible_ssh_extra_args="-o IdentitiesOnly=yes"
    ansible_python_interpreter=/usr/bin/python3
    ```

- Example of `playbook.yaml`

    ```shell
    ---
    - hosts: eth-node
      gather_facts: yes
      become: yes
      roles:
      - ethnode

    - hosts: monitor
      gather_facts: yes
      become: yes
      roles:
      - monitoring
    ```

### Execute ansible

- Execute following command for successfull configuration of ETH RPC nodes with monitoring
  - `cd ~/infra/ansible`
  - `ansible-playbook -i hosts.cfg playbook.yaml -C` [dry-run]
  - `ansible-playbook -i hosts.cfg playbook.yaml -v`

### Monitoring URLs

- Gragana: http://Instance_IP:3000
  - Grafana `login` is disabled
  - List of dashboards
    - `Docker Container & Host Metrics`
    - `Execution client & Consensus client logs`
    - `Ethereum Metrics Exporter (Single)`
    - `Geth Dashboard`
    - `Prysm Dashboard`

- Prometheus: http://Instance_IP:9090
- Alerts - http://Instance_IP/alerts
  - List of alets - `InstanceDown`, `HighCPUUsage`, `HighMemoryUsage`, `HighDiskUsage`, `NumberOfPeersLow (Execution Client)`, `NumberOfPeersLow (Consensus Client)`
  - Check alert_rules.yaml file for alert rules.
- Prometheus metrics target list - http://Instance_IP:9090/targets
