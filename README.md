# Polkascan Open-Source
Polkascan Open-Source Application

## Quick deployment
### Step 1: Clone repository: 
```bash
git clone https://github.com/sora-xor/polkascan-os.git
```
### Step 2: Change directory: 
```bash
cd polkascan-os
```
### Step 3: Make sure to also clone submodules within the cloned directory: 
```bash
git submodule update --init --recursive
```
### Step 4: Change Substrate URL
Change `SUBSTRATE_RPC_URL` in `docker-compose.substrate-node-template.yml` in order to change the SORA rpc url
### Step 6: Then build mysql
```bash
docker-compose -p node-template -f docker-compose.substrate-node-template.yml up -d mysql
```
### Step 6: Then build polkascan-os
```bash
docker-compose -p node-template -f docker-compose.substrate-node-template.yml up --build
```
### Other parameters
`NETWORK_NAME` in `docker-compose.substrate-node-template.yml` for changing network name in UI, i.e. `SORA-dev`, `SORA-staging`.


## Links to applications
* Polkascan Explorer GUI: http://127.0.0.1:8080
* Monitor harvester progress: http://127.0.0.1:8080/sora/harvester/admin
* Harvester Task Monitor: http://127.0.0.1:5555
* Polkadot JS Apps: http://127.0.0.1:8081


## Add custom types for SORA
* Modify `harvester/app/type_registry/default.json` to match the introduced types of the custom chain
* Truncate `runtime` and `runtime_*` tables on database
* Start harvester
* Check http://127.0.0.1:8080/sora/runtime-type if all type are now correctly supported
* Monitor http://127.0.0.1:8080/sora/harvester/admin if blocks are being processed and try to restart by pressing "Process blocks in harvester queue" if process is interupted.

## System requrements
Memory: >2GB (more is better), 

Storage: >5GB (SSD is better), 

Processor: more and faster cores is better (Intel i3).

Software requirements: Git, Docker & Docker Compose.

Deployment of the enriched Substrate Interface has been tested on Mac, Linux and Windows.


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
    type_registry_preset='default',
    type_registry=load_type_registry_file('harvester/app/type_registry/custom_types.json'),
)

block_hash = substrate.get_block_hash(block_id=3899710)

extrinsics = substrate.get_block_extrinsics(block_hash=block_hash)

print('Extrinsincs:', json.dumps([e.value for e in extrinsics], indent=4))

events = substrate.get_events(block_hash)

print("Events:", json.dumps([e.value for e in events], indent=4))
```

One can use the `harvester/query.py` file. It can be run with:
```bash
cd harvester
python -m pip install -r requirements.txt
python query.py
```
