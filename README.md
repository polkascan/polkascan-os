# Polkascan PRE
Polkascan PRE Main Application

## Run application

* Make sure to also clone submodules within the cloned directory: 
```bash
git submodule update --init --recursive
```
* During the first run let MySQL initialize (wait for about a minute)

```bash
docker-compose -f docker-compose.yml up -d mysql
```
* Then build the other docker containers
```bash
docker-compose -f docker-compose.yml up --build
```

This will harvest blocks from a new local network, for existing networks replace the mentioned docker-compose.yml with:

* docker-compose.alexander.yml for Alexander test network
* docker-compose.edgeware.yml for Edgeware test network
* docker-compose.robonomics.yml for Robonomics test network

## Links

* Polkascan Explorer GUI: http://127.0.0.1:8080
* Harvester task monitor: http://127.0.0.1:5555
* Harvester progress: http://127.0.0.1:8080/harvester/admin
