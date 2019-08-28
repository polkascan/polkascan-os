# Polkascan PRE
Polkascan PRE Main Application

## Run application

* Make sure to also clone submodules within the cloned directory: 
```bash
git submodule update --init --recursive
```
* During the first run let MySQL initialize (wait for about a minute)

```bash
docker-compose -p dev -f docker-compose.yml up -d mysql
```
* Then build the other docker containers
```bash
docker-compose -p dev -f docker-compose.yml up --build
```

This will harvest blocks from a new local network, for existing networks replace the mentioned docker-compose.yml with:

* Kusama network
```bash
docker-compose -p kusama -f docker-compose.kusama.yml up --build
```
* Alexander test network
```bash
docker-compose -p alexander -f docker-compose.alexander.yml up --build
```
* Edgeware test network
```bash
docker-compose -p edgeware -f docker-compose.edgeware.yml up --build
```
* Robonomics test network
```bash
docker-compose -p robonomics -f docker-compose.robonomics.yml up --build
```
* Joystream test network
```bash
docker-compose -p joystream -f docker-compose.joystream.yml up --build
```

## Links

* Polkascan Explorer GUI: http://127.0.0.1:8080
* Harvester task monitor: http://127.0.0.1:5555
* Harvester progress: http://127.0.0.1:8080/harvester/admin
