# The Mediator

**Delivers intercepted traffic to the LEAs**

* Receives the intercepts from the collectors
* Establishes and maintains handovers to LEAs
* Ensure that the intercepted traffic goes to the right LEA

## Mediator Configuration

### 1. Container Login

```bash
docker exec -it openli-mediator /bin/bash
```

### 2. Query the container's IP Address on the openli-lab network

```bash
ip addr list eth1
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/0a01b617-aed5-4203-957c-43c87ade6733)

In my case, the IP address of `eth1` is `172.18.0.3`.

### 3. Configuration

```bash
vim /etc/openli/mediator-config.yaml
```

```bash
operatorid: <OPID>

listenport: <MEDLISTENPORT>
listenaddr: <MEDIP>

provisioneraddr: <PROVIP>
provisionerport: <MEDIATORPORT>

mediatorid: <MEDIDNUM>

etsitls: no
```

#### A. Configuring Listening IP

Replace all instances of `<MEDIP>` with the correct IP Address i.e. `172.18.0.3`. 


```bash
operatorid: <OPID>

listenport: <MEDLISTENPORT>
listenaddr: 172.18.0.3

provisioneraddr: <PROVIP>
provisionerport: <MEDIATORPORT>

mediatorid: <MEDIDNUM>

etsitls: no
```

#### B. Configuring Provisioner Connection

Replace all instances of `<PROVIP>` and `<MEDIATORPORT>` with the Provisioner IP Address(`172.18.0.2`) and `9002` port number respectively, as configured in the previous section.

```bash
operatorid: <OPID>

listenport: <MEDLISTENPORT>
listenaddr: 172.18.0.3

provisioneraddr: 172.18.0.2
provisionerport: 9002

mediatorid: <MEDIDNUM>

etsitls: no
```

#### C. Listening for Collectors

Choose a distinctive port for the listening service.
* Collectors will connect to this mediator on this port -- **12009**

#### D. Configuring Listener

```bash
operatorid: <OPID>

listenport: 12009
listenaddr: 172.18.0.3

provisioneraddr: 172.18.0.2
provisionerport: 9002

mediatorid: <MEDIDNUM>

etsitls: no
```

#### E. Mediator Identity Fields 

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/5b0bb1cc-5ec4-4474-80e2-2f724ea5864a)

#### F. Configuring Identity

```bash
operatorid: WAND

listenport: 12009
listenaddr: 172.18.0.3

provisioneraddr: 172.18.0.2
provisionerport: 9002

mediatorid: 1

etsitls: no
```

### 4. Logging with rsyslog

```bash
cp /etc/openli/rsyslog.d/10-openli-mediator.conf /etc/rsyslog.d/

# This step is needed only when using the lab container
stop_rsyslog.sh
service rsyslog restart
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/1256dbb0-d54b-4153-9b46-10d665c27ae6)

### 5. Starting the Mediator

```bash
service openli-mediator start
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/bd3e9dc9-4ee7-40c5-b89e-ae2ffacd7f8d)

Examine Logs for any obvious error messages: `less /var/log/openli/mediator.log`.

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/4a358228-b3ee-4924-ae34-6d8b272c314b)

### Stopping the Mediator

```bash
# on the lab container 
service openli-mediator stop
stop_mediator.sh
```

[Collector Configuration](./collector.md)


