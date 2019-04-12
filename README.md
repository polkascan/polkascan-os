# Polkascan PRE
Polkascan PRE Main Application

## Run application (Prebuilt Docker Hub images)

* During the first run let MySQL initialize (wait for 30 seconds)

```bash
docker-compose up -d mysql
```

* Then start the other docker containers
```bash
docker-compose up
```

## Run application (Build from source)

* Make sure to also clone submodules within the cloned directory: 
```bash
git submodule update --init --recursive
```
* During the first run let MySQL initialize (wait for 30 seconds)

```bash
docker-compose -f docker-compose.dev.yml up -d mysql
```
* Then build the other docker containers
```bash
docker-compose -f docker-compose.dev.yml up --build
```
## Links

* Polkascan Explorer GUI: http://127.0.0.1:8080
* Harvester task monitor: http://127.0.0.1:5555
* Harvester progress: http://127.0.0.1:8080/harvester/admin