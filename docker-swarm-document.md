# Docker Swarm Quick Start

## Initialize a swarm
The first machine to execute this command is considered the Manager
```
$ docker swarm init --advertise-addr 192.168.0.70
Swarm initialized: current node (bvz81updecsj6wjz393c09vti) is now a manager.

To add a worker to this swarm, run the following command:

docker swarm join --token SWMTKN-1-3i9ipz42jq34t56gg1xsrsjzqfovnzi8tml8nup0h4nimif3ik-8nkoxv691ngfdnjzcvpg0qmnv 192.168.0.70:2377
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
## Join a node to swarm as a worker

```
$ docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-1awxwuwd3z9j1z3puu7rcgdbx 172.17.0.2:2377
This node joined a swarm as a worker.
```
## Join a node to swarm as a manager

```
$ docker swarm join --token SWMTKN-1-3i9ipz42jq34t56gg1xsrsjzqfovnzi8tml8nup0h4nimif3ik-a8xkhidqxfczjhenrxpoygzfi 192.168.0.70:2377 

```

## Show token for join a node as a worker

```
$ docker swarm join-token worker
To add a worker to this swarm, run the following command:

docker swarm join --token SWMTKN-1-3i9ipz42jq34t56gg1xsrsjzqfovnzi8tml8nup0h4nimif3ik-8nkoxv691ngfdnjzcvpg0qmnv 192.168.0.70:2377
```

## Show token for join a node as a manager
```
$ docker swarm join-token manager

To add a worker to this swarm, run the following command:

docker swarm join --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c 192.168.99.100:2377
```

## Show Node Status
```
$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
dkp8vy1dq1kxleu9g4u78tlag    worker1   Ready   Active        
dvfxp4zseq4s0rih1selh0d20 *  manager   Ready   Active        Leader
```
## Leave the swarm
```
$ docker swarm leave --force

Node left the default swarm.
```

## Convert worker node to manager 
```
$ docker node promote DB  
```

## Convert manager node to worker
```
$ docker node demote DB  
```

## Remove a worker node from swarm
```
$ docker node rm [Hostname]

Node left the default swarm
```

## Remove a manager node from swarm
First manager node should convert to worker then remove it.
```
$ docker node rm [Hostname]

Node left the default swarm
```

## Deploy a stack to a swarm 
When running Docker Engine in swarm mode, you can use docker stack deploy to deploy a complete application stack to the swarm. The deploy command accepts a stack description in the form of a Compose file.

The docker stack deploy command supports any Compose file of version “3.0” or above. If you have an older version, see the [upgrade guide.](https://docs.docker.com/compose/compose-file/compose-versioning/#upgrading)

***NOTE***

***Before Deploying a stack, make sure that the Docker image and docker-compose file are present on all machines and are in the same condition.***

```
$ docker stack deploy -c /opt/wildfly_docker/docker-compose.yml wildfly
Creating network wildfly_default
Creating service wildfly_wildfly
```
### Show Stack status
```
$ docker stack ls
NAME      SERVICES   ORCHESTRATOR
wildfly   1          Swarm
```
## Show Latest deploy status on stack
```
$ docker stack ps wildfly
ID             NAME                IMAGE                          NODE      DESIRED STATE   CURRENT STATE            ERROR     PORTS
s4ld80rw8jbn   wildfly_wildfly.1   bitnami/wildfly:22-debian-10   DB        Running         Running 49 seconds ago             
```
### Remove Stack
````
$ docker stack rm wildfly
Removing service wildfly_wildfly
Removing network wildfly_default
````

## Add manager nodes for fault tolerance
You should maintain an odd number of managers in the swarm to support manager node failures. Having an odd number of managers ensures that during a network partition, there is a higher chance that the quorum remains available to process requests if the network is partitioned into two sets. Keeping the quorum is not guaranteed if you encounter more than two network partitions.

| Swarm Size | Majority | Fault Tolerance |
|:-------------:|:-------------:|:-----:|
| 1 | 1 | 0 |
| 3 | 2 | 1 |
| 5 | 3 | 2 |
| 7 | 4 | 3 |
| 9 | 5 | 4 |

## Monitor swarm health
```
$ docker node inspect LB1 --format "{{ .ManagerStatus.Reachability }}"   
reachable

$ docker node inspect LB1 --format "{{ .Status.State }}"       
ready
```
## Docker service update
Generally, you do not need to force the swarm to rebalance its tasks. When you add a new node to a swarm, or a node reconnects to the swarm after a period of unavailability, the swarm does not automatically give a workload to the idle node. This is a design decision
```
$ docker service ls
ID             NAME              MODE         REPLICAS   IMAGE         PORTS
yko3hkczhcsn   wildfly_wildfly   replicated   1/1        wild:latest   *:8080->8080/tcp, *:9990->9990/tcp

$ docker service update wildfly_wildfly
wildfly_wildfly
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 
```
## Portainer
Portainer is the definitive open source container management tool for Kubernetes, Docker, Docker Swarm and Azure ACI. It allows anyone to deploy and manage containers without the need to write code.

### Deploying a new stack on portainer
You can deploy a new stack from Portainer using the following options:
- Web editor: Using our Web Editor, you will capable to define the services for this stack using a docker-compose format.
- Upload: If you're already have a stack.yml file, you can upload from your computer to deploy that stack.
- Git Repository: You can use a docker-compose format file hosted in Github.
- Custom Template: If you already created a template of stacks, you can deploy from this option.

**[Click Here](https://documentation.portainer.io/v2.0/stacks/create/) To See Stack Deploy Method Document**
