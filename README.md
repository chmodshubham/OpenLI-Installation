# OpenLI-Installation

OpenLI can only be run on machines that are running Linux, there are no OpenLI binaries for Windows or Mac nor there are any long-term plans to build OpenLI for these OS.

Best way to install and maintain OpenLI - **Automatic dependency management**

## Pre-requisite

```bash
sudo apt-get install -y apt-transport-https curl
```

## Installation

WAND maintains an (unofficial) Debian & Ubuntu apt repository on [Cloudsmith](https://cloudsmith.io/~wand/repos/openli/packages/) where we make packaged versions of our software available to the public.

#### 1. To enable the OpenLI repository on Debian or Ubuntu, run the following commands:

```bash
curl -1sLf 'https://dl.cloudsmith.io/public/wand/libwandio/cfg/setup/bash.deb.sh' | sudo -E bash
curl -1sLf 'https://dl.cloudsmith.io/public/wand/libwandder/cfg/setup/bash.deb.sh' | sudo -E bash
curl -1sLf 'https://dl.cloudsmith.io/public/wand/libtrace/cfg/setup/bash.deb.sh' | sudo -E bash
```

> **Note**: Browse these shell scripts before running them to make sure that they are safe.

#### 2. Add the WAND repository for OpenLI itself:

```bash
curl -1sLf 'https://dl.cloudsmith.io/public/wand/openli/cfg/setup/bash.deb.sh' | sudo -E bash
```

#### 3. Update `apt` to have the latest info on which package exists:

```bash
sudo apt-get update
```

#### 4. Install desired OpenLI components:

```bash
sudo apt-get install openli-mediator
sudo apt-get install openli-provisioner
sudo apt-get install openli-collector
```

> **Note**: For testing, installing all the components on the same host is OK. In real OpenLI deployment, each package would probably be installed on a different host.

## What is installed

#### 1. OpenLI binaries end up in `/usr/bin`.

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/37656711-0317-45c9-89d6-3cf1db458209)

#### 2. OpenLl services are registered with `systemd`.

```bash
sudo service openli-provisioner status
sudo service openli-mediator status
sudo service openli-collector status
```

#### 3. Documentation is installed in `/usr/share/doc/openli`.

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/7e8a141c-95dd-46fb-88f8-b24457df83d5)

#### 4. OpenLI configuration files.

OpenLI configuration files must be placed in the `/etc/openli/` directory which will be created when the first component package is installed.

- Provisioner: `/etc/openli/provisioner-config.yaml` 

- Collector: `/etc/openli/collector-config.yaml`

- Mediator: `/etc/openli/mediator-config.yaml`

- Active intercept configuration: `/etc/openli/running-intercept-config.yaml`

> **Note**: Example configs are pre-installed - Named differently to avoid accidental use.

- Rsyslog config for OpenLI is installed in `/etc/openli/rsyslog.d`
  - Move this config into `/etc/rsyslog.d` to enable
  - Restart the `rsyslog` daemon using the `sudo systemctl restart rsyslog` command.
  - OpenLI will then log to `/var/log/openli`. Otherwise, all OpenLI components log into `/var/log/messages`.
 
### Updating Installed Packages:

```bash
sudo apt update && sudo apt upgrade
```

### Removing Installed Packages:

```bash
sudo apt remove --purge openli-collector
sudo apt remove --purge openli-provisioner
sudo apt remove --purge openli-mediator
```




