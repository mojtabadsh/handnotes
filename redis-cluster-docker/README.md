# REDIS Cluster by Docker

In this scenario we are going to deploy Redis as a cluster into standalone node.

### Make the following structure directories and files

```ini
redis-cluster
├── docker-compose.yml
└── redis-data
    ├── redis_0
    │   ├── data
    │   └── redis.conf
    ├── redis_1
    │   ├── data
    │   └── redis.conf
    ├── redis_2
    │   ├── data
    │   └── redis.conf
    ├── redis_3
    │   ├── data
    │   └── redis.conf
    ├── redis_4
    │   ├── data
    │   └── redis.conf
    └── redis_5
        ├── data
        └── redis.conf
```



### Redis Config File (redis.conf)

Each node must have a separate **Port** and **IP** configuration

#### Important Notice

- ###### When a application written in Java(Spring Framework) connects to a Redis cluster, it first connects to a node of the cluster and receives cluster information. If these configurations are not used, Docker returns its internal IP's to the application, which causes problems in connecting to Redis.

```ini
cluster-announce-ip x.x.x.x 
cluster-announce-port 6370 
```

- ###### The default management port of redis is 10000 + port (ex: REDIS port 6379 - cluster port: 16379)

- ###### **For proper communication between redis cluster nodes, we created a network in Docker so that all nodes in a network can access each other**

### UP and Running Cluster 

Run blow command to up cluster, check health and log

```bash
$ docker-compose up -d
$ docker-compose ps
$ docker-compose logs -f
```

To create cluster get into one of  container and run this command

```bash
$ docker exec -it redis_0 /bin/sh
$ redis-cli --cluster create IP:6370 IP:6371 IP:6372 IP:6373 IP:6374 IP:6375 --cluster-replicas 1
```

To check cluster nodes status get into a one redis container and run this command

```bash
$ docker exec -it redis_0 /bin/sh
$ redis-cli -p 6370 cluster nodes
```

