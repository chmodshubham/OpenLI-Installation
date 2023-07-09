# The Provisioner

**Central orchestrator for OpenLI**
* All other components connect to the provisioner
* Receive instructions

**Goal**: get the provisioner listening for client connections


#### 1. Container Login

```bash
docker exec -it openli-provisioner /bin/bash
```

#### 2. Query the container's IP Address on the openli-lab network

```bash
ip addr list eth1
```

