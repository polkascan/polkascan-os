# Polkascan Open-Source
Polkascan Open-Source Application

## Run application for a development network
The following steps will run a full Polkascan-stack that harvests blocks from a new local network.

### Step 1: Clone repository: 
```bash
git clone https://github.com/polkascan/polkascan-pre.git
```
### Step 2: Change directory: 
```bash
cd polkascan-pre
```
### Step 3: Check available releases: 
```bash
git tag
```
### Step 4: Checkout latest releases: 
```bash
git checkout v0.x.x
```
### Step 5: Make sure to also clone submodules within the cloned directory: 
```bash
git submodule update --init --recursive
```
### Step 6: During the first run let MySQL initialize (wait for about a minute)
```bash
docker-compose -p dev -f docker-compose.yml up -d mysql
```
### Step 7: Then build the other docker containers
```bash
docker-compose -p dev -f docker-compose.yml up --build
```

## Run application for existing public networks
For existing public networks use the following commands in step 6 & 7 respectively:

### Kusama
```bash
docker-compose -p kusama -f docker-compose.kusama.yml up -d mysql
docker-compose -p kusama -f docker-compose.kusama.yml up --build
```

## Links to applications

* Polkascan Explorer GUI: http://127.0.0.1:8080
* Harvester Task Monitor: http://127.0.0.1:5555
* Harvester Status: http://127.0.0.1:8080/harvester/admin
* Polkadot JS Apps: http://127.0.0.1:8081

## Cleanup Docker
Use the following commands with caution to cleanup your Docker environment.

### Prune images
```bash
docker system prune
```

### Prune images (force)
```bash
docker system prune -a
```

### Prune volumes
```bash
docker volume prune
```
