# Polkascan Open-Source
Polkascan Open-Source Application

## Use custom Substrate template

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
docker-compose -p gesell -f docker-compose.substrate-node-template.yml up -d mysql
```
### Step 7: Then build the other docker containers
```bash
docker-compose -p gesell -f docker-compose.substrate-node-template.yml up --build
```

## Links to applications
* Polkascan Explorer GUI: http://127.0.0.1:8080
* Monitor harvester progress: http://127.0.0.1:8080/kusama/harvester/admin
* Harvester Task Monitor: http://127.0.0.1:5555
* Polkadot JS Apps: http://127.0.0.1:8081

## Other networks

* Polkadot: Use `docker-compose.polkadot-quick.yml`, `docker-compose.polkadot-public.yml` and `docker-compose.polkadot-full.yml`
* Substrate Node Template (https://github.com/substrate-developer-hub/substrate-node-template): Use `docker-compose.substrate-node-template.yml`

## Add custom types for Substrate Node Template

* Modify `harvester/app/type_registry/substrate-node-template.json` to match the introduced types of the custom chain
* Truncate `runtime` and `runtime_*` tables on database:
To truncate, first enter the running mysql container: docker exec -it mysql_container_name mysql -uroot -p
Change to the database defined in the docker-compose (by default polkascan): USE polkascan
See all tables: SHOW tables;
Truncate each table with runtime, example: truncate runtime; 
* Start harvester
* Check http://127.0.0.1:8080/node-template/runtime-type if all type are now correctly supported
* Monitor http://127.0.0.1:8080/node-template/harvester/admin if blocks are being processed and try to restart by pressing "Process blocks in harvester queue" if process is interupted.

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

### API specification

The Polkascan API implements the https://jsonapi.org/ specification. An overview of available endpoints can be found here: https://github.com/polkascan/polkascan-pre-explorer-api/blob/master/app/main.py#L60

## Troubleshooting

When certain block are not being processed or no blocks at all then most likely there is a missing or invalid type definition in the [type registry](https://github.com/polkascan/polkascan-pre-harvester/blob/c5f544ad631e3754ba1e818a26b7aac1ef11f287/app/type_registry/custom_types.json).

Some steps to check:

* The Substrate node should be started in archive mode (`--pruning archive`)
* Be sure to regularly update, also the Git submodules: `git submodule update --init --recursive` for latest version of py-scale-codec
* Check which blocks are not being processed and try to restart manually at: http://127.0.0.1:8080/<NETWORK_NAME>/harvester/admin
* Check Celery queue for more details about failing blocks: http://127.0.0.1:5555/tasks?state=FAILURE
* Check and match types in [./harvester/app/type_registry/custom_types.json](https://github.com/polkascan/polkascan-pre-harvester/blob/c5f544ad631e3754ba1e818a26b7aac1ef11f287/app/type_registry/custom_types.json) with added types in Substrate 

You can also dive into Python to pinpoint which types are failing to decode:

```python
import json
from scalecodec.type_registry import load_type_registry_file
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(
    url='ws://127.0.0.1:9944',
    type_registry_preset='substrate-node-template',
    type_registry=load_type_registry_file('harvester/app/type_registry/custom_types.json'),
)

block_hash = substrate.get_block_hash(block_id=3899710)

extrinsics = substrate.get_block_extrinsics(block_hash=block_hash)

print('Extrinsincs:', json.dumps([e.value for e in extrinsics], indent=4))

events = substrate.get_events(block_hash)

print("Events:", json.dumps([e.value for e in events], indent=4))
```
