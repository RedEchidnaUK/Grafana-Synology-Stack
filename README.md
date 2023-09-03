# Grafana Synology Stack
## Overview
This is an example/demo Gafana Stack consisting of the Grafana Dashboard, Grafana Mimir, Grafana Loki and Grafana Agent. It is intended that this stack is run in Docker on a Synology NAS. **Knowledge of Docker and Docker Compose are required.**

A Minio instance is used for the S3 compatible storage and NGINX is used as a basic proxy/load balancer.

### Notes:
This stack was thrown together as a proof-of-concept over a few days to demonstate some of the Grafana Stack/products running in a monolithic architecture. It is **not designed to be used in production**, but may give you a basic setup you can build from. For example, you could easily improve the security by adding TLS and basic authentication to the NGINX proxy.

Unless you modify the **docker-compose.yaml** file, the following ports must be free on the Synology NAS
   * 3000 - Grafana Dashboard
   * 3100 - Grafana Loki
   * 9009 - Grafana Mimir  

## How to get up and running
1. Copy all of the folders to a location on the Synology NAS that will contain all of the persistent Docker container files e.g. A folder called **docker/grafana** on the main volume, aka **volume1/docker/grafana** when viewed via the command line. Other locations may be used, but the **docker-compose.yaml** file will require updating.

2. Configure the Synology NAS to expose data via SNMP v2 using a community called **public**.

3. Edit **agent/config/linux/agent.yaml** and replace **my-nas-ip** with the Synology NAS IP address.

4. Run **docker-compose.yaml up** from the command line or via Portainer etc. If using Portainer see the known issues below.

## Known Issues/Limitations
### Portainer on Synology
* When starting/stopping the stack via Portainer running on Synology, the stack will sometimes report it has failed to start/stop, but if you wait it actually starts/stops fine. 
* Occasionaly components/containers of the stack will fail to start, but will start fine when manually started.

Both of these issues may be realted to a lack of health-checks etc. on the continers so things are started out of order or take too long to startup etc. 

### Grafana Agent Container > 0.34.0
* Running a Grafana Agent container greater than 0.34.0 results in the container exiting due to an issue with the SNMP config file. Attempts to fix this, even with Grafana's latest example SNMP file, casues the container to crash with a memory exception error after about 5 minutes.
