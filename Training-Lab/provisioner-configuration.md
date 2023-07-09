# The Provisioner

**Central orchestrator for OpenLI**
* All other components connect to the provisioner
* Receive instructions

**Goal**: get the provisioner listening for client connections

## Provisioner Configuration

### 1. Container Login

```bash
docker exec -it openli-provisioner /bin/bash
```

### 2. Query the container's IP Address on the openli-lab network

```bash
ip addr list eth1
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/c2d672a2-e65f-4df3-9369-ad88a39c501b)

In my case, the IP address of `eth1` is `172.18.0.2`.

### 3. Configuration

```bash
vim /etc/openli/provisioner-config.yaml
```

```bash
clientport: <COLLECTORPORT>
clientaddr: <PROVIP>
updateport: <RESTAPIPORT>
updateaddr: <PROVIP>
mediationport: <MEDIATORPORT>
mediationaddr: <PROVIP>
```

#### A. Configuring Listening IPs

Replace all instances of `<PROVIP>` with the correct IP Address i.e. `172.18.0.2`.

```bash
clientport: <COLLECTORPORT>
clientaddr: 172.18.0.2
updateport: <RESTAPIPORT>
updateaddr: 172.18.0.2
mediationport: <MEDIATORPORT>
mediationaddr: 172.18.0.2
```

#### B. Configuring Ports

Choose 3 distinct ports for listening services(any port no. > 1024):
* One for the REST API -- **8080**
* One for collectors to connect to -- **9001**
* One for mediators to connect to -- **9002**

Update the `provisioner-config.yaml` with the port numbers:

```bash
vim /etc/openli/provisioner-config.yaml
```

```bash
clientport: 9001
clientaddr: 172.18.0.2
updateport: 8080
updateaddr: 172.18.0.2
mediationport: 9002
mediationaddr: 172.18.0.2
```

### 4. Logging with rsyslog

```bash
cp /etc/openli/rsyslog.d/10-openli-provisioner.conf /etc/rsyslog.d/

# this step is needed only when using the lab container
stop_rsyslog.sh
service rsyslog restart
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/2f5a8499-bcee-43a9-97d7-212defd6f682)

> **Note**: Unfortunately `docker` and `systemd` has some incompatibilities that mean `systemd` cannot be used to stop a service running within a container. This script is part of a specific glitch in the lab containers and is not a part of the regular openli configuration.

### 5. Starting the Provisioner

```bash
service openli-provisioner start
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/ea51cfa7-a121-40a4-9915-2365e2fd335c)

Examine Logs for any obvious error messages: `less /var/log/openli/provisioner.log`.

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/d2197446-38bc-47df-b212-f9b2e6b1588b)

## Stopping the Provisioner

```bash
# on the lab container 
service openli-provisioner stop
stop_provisioner.sh
```

[Mediator Configuration](./mediator-configuration.md)
