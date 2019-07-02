# raspIOTstackd
docker stack for getting started on IOT on the Raspberry PI

This Docker stack consists of:
  *norered
  *Grafana
  *influxDB

## Before you start
I followed these instructions to setup docker on my pi
https://howchoo.com/g/nmrlzmq1ymn/how-to-install-docker-on-your-raspberry-pi

to install docker-compose
```
sudo apt update && sudo apt install -y docker-compose
```

Note: when I installed docker-compose it is not the latest version.
It only supports Version '2' of the compose instructions and therefore some of the more advanced instructions have been omitted

## Folder permissions
when docker starts the compose for the first time it creates the folders for linking the volumes
there is an issue with Grafana where a different user and group is used run the `folderfix.sh'

```
sudo chmod +x ./folderfix.sh
sudo ./folderfix.sh
```

## Starting and Stopping containers
to start the stack navigate to the folder containing the docker-compose.yml file

run the following
`docker-compose up -d`

to stop
`docker-compose down`

docker deletes the containers with the docker-compose down command. However because the compose file specifies volumes the data is stored in persistent folders on the host system

## Updating the images
if a new version of a container it is simple to update it.
use the  `docker-compose down` command to stop the stack

pull the latest version from docker hub with one of the following commands

```
docker pull grafana/grafana:latest
docker pull influxdb:latest
docker pull nodered/node-red-docker:rpi
```

## Networking
The compose instruction creates a internal network for the containers to communicate in.
It also creates a "DNS" the name being the container name.
When you need to specify the address of your influxdb it will not be 127.0.0.1:8086 ! It will be influxdb:8086
Similarly inside the containers the containers talk by name however if you need to interact with it you do if via your pi's ip e.g. 192.168.0.n:3000