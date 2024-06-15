## Initia-Monitor

Initia-Monitor: This tool is to monitor the status of the node/validator.

### This tool is intended to monitor and alert on telegram any problem related to the cosmos node:

- Server down
- Out of memory (<10%)
- Out of disk space (<10%)
- Out of disk space within 24h
- High CPU load (>85%)

### Cosmos-based validator alerts

- Missing blocks
- Degraded syncing (sync less than 40 blocks in last 5 min)
- Low peers count (<5)

### Used containers from Docker

- [grafana sever](https://hub.docker.com/r/grafana/grafana)
- [node_exporter](https://hub.docker.com/r/prom/node-exporter)
- [prometheus](https://hub.docker.com/r/prom/prometheus)
- [alertmanager](https://hub.docker.com/r/prom/alertmanager)

### Login Data http://<your_server_ip>:3000

 - username: admin
 - default password: admin
 
## Coommands for run this monitoring tool:

 1. Install Docker first
```
bash <(curl -s https://raw.githubusercontent.com/nodejumper-org/monitoring-tool/main/utils/install_docker.sh)
```
![1](https://github.com/VuzzyM/initia-monitor/assets/66425682/64bb4955-4262-4daa-9f0b-323fcc565bac)

2. Clone the repo
```
cd ~
git clone https://github.com/nodejumper-org/monitoring-tool.git 
```
![2](https://github.com/VuzzyM/initia-monitor/assets/66425682/39ffee41-813e-410e-8e0b-8e83fac1ca91)

3. Create configuration files from examples
```
cd monitoring-tool
cp prometheus/prometheus.yml.example prometheus/prometheus.yml
cp alertmanager/config.yml.example alertmanager/config.yml
```
![3](https://github.com/VuzzyM/initia-monitor/assets/66425682/7379f23b-3d72-4641-b0c6-cdf78f34ef57)

4. Start containers
```
sudo docker compose up -d
```
![5](https://github.com/VuzzyM/initia-monitor/assets/66425682/c1b96bf7-2483-4032-9ed8-49f5f1da82a2)


  ## How to set up
 ### Servers to monitor
Add your servers with installed [node_exporter](https://github.com/prometheus/node_exporter) or installed cosmos-based node with enabled prometheus port to file `prometheus/prometheus.yml`

![7](https://github.com/VuzzyM/initia-monitor/assets/66425682/29f03cc1-e08b-41d9-9ea6-3508ccfd04c4)

```
  # example for servers with node_exporter installed
  - job_name: "my-servers"
    static_configs:
    - targets: ["172.0.0.1:9100"]
      labels:
        instance: "server1"
    - targets: ["172.0.0.2:9100"]
      labels:
        instance: "server2"
    
  # example for servers with node_exporter and cosmos-based node installed
  - job_name: "cosmos-validator-nodes"
    static_configs:
    - targets: ["192.0.0.1:9100","192.0.0.1:26660"]
      labels:
        instance: "validator1"
    - targets: ["192.0.0.2:9100","192.0.0.2:26660"]
      labels:
        instance: "validator2"
    - targets: ["192.0.0.3:9100","192.0.0.3:26660"]
      labels:
        instance: "validator3"
```

## Don't forgot to turn on Prometheus metrics

```
sudo nano $HOME/.initia/config/app.toml
```

![8](https://github.com/VuzzyM/initia-monitor/assets/66425682/b518df67-551f-4ee3-9c56-af5408c85624)

![9](https://github.com/VuzzyM/initia-monitor/assets/66425682/42ddf3ea-fe20-4315-a84d-7afa7e946b96)


### Telegram notifications
In order to enable telegram notifications, create your own bot and fill in the following fields in the file <b>alertmanager/config.yml</b>
```
chat_id=1111111                 # your telegram user id
bot_token=11111111:AAG_XXXXXXX  # your telegram bot token
```

## How to update
How to update the docker container
```
cd monitoring-tool
sudo docker-compose down
git pull
sudo docker-compose pull
sudo docker-compose up -d
```

The source of the tool can be found here - [nodejumper](https://github.com/nodejumper-org/monitoring-tool?tab=readme-ov-file)
