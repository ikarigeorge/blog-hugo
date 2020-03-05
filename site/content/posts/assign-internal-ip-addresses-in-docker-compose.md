---
title: Assign internal IP addresses in docker compose
date: 2020-03-05T07:57:07.755Z
tags:
  - docker
  - docker-compose
---
Sometimes it is needed to fix IP addresses in some containers declared in a docker compose file. To achieve that it is needed to configure the network. Lets suppose the network is called mynetwork and we want to assign ip addresses in the "172.20.0.X" range:

```
networks:
  mynetwork:
    ipam:
      config:
        - subnet: 172.20.0.0/24
```

Then, for the desired services you can assign a fixed ip address, for example:
```
services:
  myservice:
    image: myimage:latest
    networks:
      mynetwork:
        ipv4_address: 172.20.0.10
```

