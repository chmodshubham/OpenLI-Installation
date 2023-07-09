# OpenLI Training Lab

Sandbox environment for experimenting with OpenLI:

- No special hardware is required, just a Linux server/VM or laptop
- Docker containers for each component
- A fake LEA that will receive and output intercepts
- Does not require a real network tap, instead use `pcap` traces that can be replayed into the lab network to emulate the real-world traffic that can be intercepted.

## Pre-requisite

#### 1. Install `docker`

```bash
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# to run docker without sudo:
sudo usermod -aG docker ${USER}
su - ${USER}
sudo chmod 666 /var/run/docker.sock
```

## Lab Setup

#### 1. Build the Training Lab

```bash
git clone https://github.com/wanduow/openli-training-lab.git
cd openli-training-lab/
./setup.sh
```

After this script, 4 containers are now running on the host:

* Provisioner
* Collector
* Mediator
* A "pretend" LEA

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/3836a261-e8b3-4f06-9429-d5b92a57395a)

`eth0`: access to upstream internet via docker-host. e.g. for installing software packages
`eth1`: inter-component communication
`eth2` on mediator: direct connection to the LEA
`eth2` on collector: interface for packet capture(e.g. from the 5G NFs)

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/4bfbf907-4638-47e9-8c35-e10e2af3791c)

#### 2. Container Login

```bash
docker exec -it openli-collector /bin/bash
docker exec -it openli-mediator /bin/bash
docker exec -it openli-provisioner /bin/bash
docker exec -it openli-agency /bin/bash
```

### Container Removal

```bash
docker stop openli-agency
docker stop openli-collector
docker stop openli-mediator
docker stop openli-provisioner
```

Running `setup.sh` again will stop and recreate the containers.


[Provisioner Configuration](./provisioner-configuration.md)