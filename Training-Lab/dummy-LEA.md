# Dummy LEA

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/69400da6-8931-4d9b-b443-06993f775e60)

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/cf5e4267-cdc3-40aa-be54-e773713e3ca5)

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/2902ebd5-8378-4cb1-8e19-c33aafaa4243)

## Emulating an LEA

### 1. Container Login

```bash
docker exec -it openli-agency /bin/bash
```

### 2. Query the container's IP Address

```bash
ip addr list eth1
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/e35ebfd2-a37b-41ea-bac5-8288cfe7ea0f)

In my case, the IP address of `eth1` is `172.19.0.3`.

### 3. Select handover Ports

Choose a port number for **HI2** and **HI3** 

* **HI2** is for receiving **IRI** records from the mediator -- **41002**
* **HI3** is for receiving **CC** records from the mediator -- **41003**

### 4. Start HI3 Handover

```bash
tracepktdump etsilive:172.19.0.3:41003
```

No output just yet
* Waiting for the mediator to connect to it.

### 5. Start HI2 Handover

On a 2nd shell on the `openli-agency` container, run another instance of `tracepktdump` on the HI2 port.

```bash
tracepktdump etsilive:172.19.0.3:41002
```

### 6. RestAPI for Agencies

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/a22fdd4c-5c8e-4070-9dd0-116c1ff6a6ee)

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/1ae2f70c-00c1-48c8-a682-c1859f6bdabc)

`http://172.18.0.2:8080/agency`

Example `JSON` Object for our mock agency:

```bash
{
"agencyid": "mocklea",
"hi2address": "172.19.0.3″,
"hi3address": "172.19.0.3″,
"hi2port": "41002",
"hiзport": "41003",
"keepalivefreq": 60,
"keepalivewait": 30
}
```

Open another shell on the provisioner and run this command to `POST` the agency `JSON` object.

```bash
curl -X POST -H "Content-Type: application/json" -d '{
  "agencyid": "mocklea",
  "hi2address": "172.19.0.3",
  "hi3address": "172.19.0.3",
  "hi2port": "41002",
  "hi3port": "41003",
  "keepalivefreq": 60,
  "keepalivewait": 30
}' http://172.18.0.2:8080/agency
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/06f8020e-2d5f-47c9-976a-e505c6e7d04a)

### Handovers

Each handover is now handling 1 source.

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/158d8ea1-f5cf-4b2b-967e-6e1ec27dad89)

### Mediator Log

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/1a6679fe-aa06-4fbf-bc49-a9d075b50568)

### Provisioner Log

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/2e0ea206-bc6f-467e-8edb-44e1a876c391)

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/8ce9aa60-1da5-4634-9fd6-f26056af4c29)

### More RestAPI for Agencies

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/c33e80be-486b-4881-a094-6d0fe17d22ed)

```bash
curl -X DELETE http://172.18.0.2:8080/agency/mocklea
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/c5be1b1e-b7b4-4e66-a7d9-81793d0620e2)

```bash
curl -X GET http://172.18.0.2:8080/agency/mocklea
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/dca3bda8-92e5-430b-9902-c1214c5f6543)

```bash
curl -X DELETE http://172.18.0.2:8080/agency
```

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/82cdd4d3-0c06-4def-ab6d-78e59c55c1e0)

```bash
curl -X PUT -H "Content-Type: application/json" -d '{"hi2port": "11002", "hi3port": "11003", "agencyid": "mocklea"}' http://172.19.0.3:8080/agency
```

### More tracepktdump tips

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/ebf9ae1e-f5f8-4964-8680-7181fd71f221)

### Other Libtrce tools

![image](https://github.com/ShubhamKumar89/OpenLI-Installation/assets/97805339/2b98240b-1e63-4bce-ad89-8c3f2c98b2a1)
