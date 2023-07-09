# The Collector

**Intercepts network traffic and encodes into ETSI format**

* Receives instructions from the provisioner
* Monitors session management protocols for target identities
* Intercepts packets that belong to an intercept target
* Labels and sequences intercepted packets о
* Forwards encoded records to the mediator

## Collector Configuration

### 1. Container Login

```bash
docker exec -it openli-collector /bin/bash
```

### 2. Query the container's IP Address on the openli-lab network

```bash
ip addr list eth1
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/dbec43c3-c6cc-49d7-b4bb-49b6f771f126)

In my case, the IP address of `eth1` is `172.18.0.4`.

### 3. Configuration

```bash
vim /etc/openli/collector-config.yaml
```

```bash
operatorid: <OPID>
networkelementid: openli-lab
interceptpointid: col001

encoderthreads: 2

inputs:
  - uri: eth2
    threads: 2
    hasher: radius
    #filter:

etsitls: no

provisioneraddr: <PROVIP>
provisionerport: <COLLECTORPORT>
```

#### A. Configuring Provisioner Connection

Replace all instances of `<PROVIP>` and `<COLLECTORPORT>` with the Provisioner IP Address(`172.18.0.2`) and `9001` port number respectively, as configured in the previous sections.

```bash
operatorid: <OPID>
networkelementid: openli-lab
interceptpointid: col001

encoderthreads: 2

inputs:
  - uri: eth2
    threads: 2
    hasher: radius
    #filter:

etsitls: no

provisioneraddr: 172.18.0.2
provisionerport: 9001
```

#### B. Collector Identity Fields

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/71a3bbb3-a8d6-4022-95d6-2903cb881b0f)

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/eb51c6c9-2c18-46b0-9809-c41867cd74dc)

#### C. Configuring Identity

```bash
operatorid: WAND
networkelementid: openli-lab
interceptpointid: col001

encoderthreads: 2

inputs:
  - uri: eth2
    threads: 2
    hasher: radius
    #filter:

etsitls: no

provisioneraddr: 172.18.0.2
provisionerport: 9001
```

### 4. Logging with rsyslog

```bash
cp /etc/openli/rsyslog.d/10-openli-collector.conf /etc/rsyslog.d/

# This step is needed only when using the lab container
stop_rsyslog.sh
service rsyslog restart
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/dac912a2-6ee3-499d-a7a2-cbde340cd7d5)

### 5. Starting the Collector

```bash
service openli-collector start
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/8797d54c-2b9e-4c9c-b71a-1d47769c6a62)

Examine Logs for any obvious error messages: `less /var/log/openli/collector.log`.

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/10c96fd2-bd80-47cb-af24-1641f6b090da)

### Stopping the Collector

```bash
# on the lab container 
service openli-collector stop
stop_collector.sh
```

[Simulating a dummy LEA](./dummy-LEA.md)