# Ethereum RPC nodes

## Architecture

1. [Architectue Diagram](https://github.com/royki/ethereum-node/blob/master/Eth-RPC.png)
2. The proposed architecture recommends deploying instances in a multi-az setup and configuring each node with eth1 and eth2 clients.
3. A load balancer will be provided to distribute incoming traffic and serve as an external URL for external access.
4. WAF needs to be applied to the load balancer before making it public. This includes as example:
5. Additionally, configurations for Route 53 and Certificate Manager are necessary to provide a DNS for users.
6. A separate monitoring node is designated to monitor all RPC nodes. This monitoring setup includes:
   1. Configuration with Prometheus, Grafana, Loki, Node Exporter, Eth metrics exporeter, Cadvisors, Alertmanager.
   2. Alerting based on resource usage and blockchain network peer numbers
   3. Alerts are sent to slack channel based on alert rules.
7. Access to RPC nodes is restricted via IP whitelisting.
8. Infra provistion and other setup from the architecture are out of the scope.
9. Monitoring for nodes are setup using Grafana, Prometheus,
10. There are 2 setup for infra configurations.
    1. Configure and run via ansible. This is more flexible and can be used to configure multiple Eth nodes with separate monitoring server.
    2. Configure and run docker-compose. This needs manual intervention inside the node. Monitoring setup is in the same eth instance.

### Configure and run via ansible

- [Check Readme for more details](<https://github.com/royki/ethereum-node/blob/master/infra/ansible/README.md>)

### Configure and run via docker-compose

- [Check Readme for more details](<https://github.com/royki/ethereum-node/blob/master/infra/docker-setup/README.md>)
