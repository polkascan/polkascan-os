# Polkascan Open-Source
Polkascan Open-Source Application

## Quick deployment
### Step 1: Clone repository: 
```bash
git clone https://github.com/polkascan/polkascan-os.git
```
### Step 2: Change directory: 
```bash
cd polkascan-os
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
### Step 6: Then build the other docker containers
```bash
docker-compose -p kusama -f docker-compose.kusama-quick.yml up --build
```

## Full deployment
The following steps will run a full Polkascan-stack that harvests blocks from a new local network.

### Step 1: Clone repository: 
```bash
git clone https://github.com/polkascan/polkascan-os.git
```
### Step 2: Change directory: 
```bash
cd polkascan-os
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
docker-compose -p kusama -f docker-compose.kusama-full.yml up -d mysql
```
### Step 7: Then build the other docker containers
```bash
docker-compose -p kusama -f docker-compose.kusama-full.yml up --build
```

## Links to applications
* Polkascan Explorer GUI: http://127.0.0.1:8080
* Monitor harvester progress: http://127.0.0.1:8080/kusama/harvester/admin
* Harvester Task Monitor: http://127.0.0.1:5555
* Polkadot JS Apps: http://127.0.0.1:8081

## Other networks

* Polkadot CC1: Use `docker-compose.polkadot-cc1-quick.yml` and `docker-compose.polkadot-cc1-full.yml`

## Custom network

* Modify `docker-compose.custom.yml` according to the specification of the custom chain
* Modify `harvester/app/type_registry/custom_types.json` to match the introduced types of the custom chain 

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
