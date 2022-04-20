## Phala PRB V2 Cluster New Deployment Tutorial

### If you are upgrading from PRB V0 to V2 [click here](https://github.com/suugee/phala-prb/blob/next/V0-V2.md)

### The cluster architecture used in this tutorial is.
![Cluster architecture used in this scenario](https://github.suugee.workers.dev/https://raw.githubusercontent.com/suugee/phala-prb/main/prb.png)

This software is suitable for small clusters of roughly 50 units; You can also run prb on a single machine. If the cluster is larger, consider node stability, load balancing solutions, or [contact Suge](# Contact Suge) to commission a custom cluster software.


# Contents
- [System Requirements](#system-requirements)
- [Node machine deployment](#1-worker-node-deployment)
  - [Install docker environment](#install-docker-environment)
  - [Start Node container](#install-docker-environment)
- [Prb machine deployment](#2-prb-node-deployment)
  - [Start Prb container](#2-prb-node-deployment)
  - [Access Monitor](#visit-monitor-httpprb1270013000)
- [Worker machine deployment](#2-prb-node-deployment)
  - [install docker environment](#worker-installation-base-environment)
  - [start pruntime container](#start-pruntime)
---
### System requirements
- Ubuntu LTS 20.04
- Docker ≥ 20.10 or later
- Docker ≥ Compose 1.29 or newer
- CPU ≥ 10 cores or more high frequency (i9 10th generation, AMD3950X, etc.)
- Memory ≥ 32G
- Hard disk ≥ 2T M.2 NVME
- Internal network Gigabit switch, Gigabit network card, Category 6 / super Category 6 Gigabit network cable
- The above is the recommended configuration for running a node+prb setup.
---
### 1. Worker node deployment

#### Install Docker environment
```
# Switch to root
sudo -i
# Install Docker
curl -sSL https://get.daocloud.io/docker | sh
# Install Docker-compose
wget -O /usr/local/bin/docker-compose https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-Linux-x86_64
chmod +x /usr/local/bin/docker-compose
```
#### After installing the base environment, download the following docker-compose configuration file and start the node.
```
# Create the directory
mkdir -p /opt/phala

# Download the yml file
wget -O /opt/phala/node.yml https://github.suugee.workers.dev/https://raw.githubusercontent.com/suugee/phala-prb/next/node.yml

docker-compose -f /opt/phala/node.yml up -d
```
- If you need to specify the Node data storage location please modify /opt/phala/node.yml and start it again.
---
### 2. Prb node deployment
#### If you deploy on the same machine as the node machine, you do not need to install Docker environment and download configuration files again.
```
# Download the yml configuration file
wget -O /opt/phala/docker-compose.yml https://github.suugee.workers.dev/https://raw.githubusercontent.com/suugee/phala-prb/next/docker-compose.yml

# Start all services with one click
docker-compose -f /opt/phala/docker-compose.yml up -d

```
#### Visit monitor: http://prb.127.0.0.1:3000

### Batch add pools and workers

- Import pools
```
curl --location --request POST 'http://path.to.monitor/ptp/proxy/Qmbz.. .RjpwY/CreatePool' \
--header 'Content-Type: application/json' \
--data-raw '{
    "pools": [
        {
            "pid": 2,
            "name": "test2",
            "owner": {
                "mnemonic": "boss. .chase"
            },
            "enabled": true,
            "realPhalaSs58": "3zieG9... .1z5g"
        }
    ]
}'
```
- Importing workers
```
curl --location --request POST 'http://path.to.monitor/ptp/proxy/Qmbz.. .RjpwY/CreateWorker' \
--header 'Content-Type: application/json' \
--data-raw '{
    "workers": [
        {
            "pid": 2,
            "name": "test-node-1",
            "endpoint": "http://path.to.worker1:8000",
            "enabled": true,
            "take": "4000000000000000"
        },
        {
            "pid": 2,
            "name": "test-node-2",
            "endpoint": "http://path.to.worker2:8000",
            "enabled": true,
            "take": "4000000000000000"
        }
    ]
}'
```

---

### 3. Worker node deployment
#### Worker installation base environment
```
sudo -i
cd ~ && wget https://raw.githubusercontent.com/suugee/phala-prb/main/worker.sh
chmod +x worker.sh && . /worker.sh
```
#### start pruntime
```
mkdir -p /opt/phala
cd /opt/phala && mv docker-compose.yml docker-compose.yml.bak
wget -O /opt/phala/docker-compose.yml https://raw.githubusercontent.com/suugee/phala-prb/main/pha_cluster.yml
docker-compose up -d pruntime
```
---

### Documentation is tedious and long to write. If this helped you, buy me a coffee by donating PHA O(∩_∩)O: `42iT6whLuSeWxqhXTYDiCyKqWwqNGWhWWmUS4Fdt4Ujnr21y`

### Commonly used commands
+ Node machine common operations
```
Start the container: docker-compose -f /opt/phala/node.yml up -d
Stop the container: docker stop node
Restart the container: docker restart node
Remove the container: docker rm node
View logs: docker logs -f node -n 100
```
+ Prb machine common operations
```
cd /opt/phala
docker-compose up -d redis io # Start the base service
docker-compose up -d data_provider #Start the data_provider service
docker-compose up -d trade #Start the trade service
docker-compose up -d lifecycle #Start the lifecycle service
docker-compose up -d monitor #Start monitor service
docker ps -a #Check container status
docker logs -f fetch #View fetch logs
docker restart container name #restart a container
```
+ Worker machine common operations
```
Start container: docker-compose up -d pruntime
Stop the container: docker stop pruntime
Remove the container: docker rm pruntime
View logs: docker logs -f pruntime
```
---
+ Official forum: https://forum.phala.network/
+ Official wiki: https://wiki.phala.network/
+ Official Github: https://github.com/Phala-Network
+ Official Telegram: https://t.me/phalaCN

### Contact Sugee
+ Sugee VX: suugee_bit
+ Suugee QQ: 6559178
+ Sugee Telegram: https://t.me/sparksure
+ Sugee Github: https://github.com/suugee/phala-prb
