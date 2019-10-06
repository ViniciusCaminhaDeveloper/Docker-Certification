# Docker Certification - Hands on
### Maintainer : Vinicius Caminha

## Domain 1: Orchestration (25% of exam)

## Content may include the following:

* Complete the setup of a swarm mode cluster with managers and worker nodes <br>
```docker 
docker swarm init --advertise-addr <IP>
docker swarm join --token TOKEN <IP>:2377
docker swarm join --token TOKEN <IP>:2377

docker node ls
```

The --advertise-addr flag configures the manager node to publish its address as 192.168.99.100. The other nodes in the swarm must be able to access the manager at the IP address.

The output includes the commands to join new nodes to the swarm. Nodes will join as managers or workers depending on the value for the --token flag.

* State the differences between running a container vs running a service<br>

```docker 
docker container run -d --name nginx nginx
docker container ls

docker service create --name nginx-service --replicas=3 nginx
docker service ls
docker service ps nginx-service
```
In short: Docker service is used mostly when you configured the master node with Docker swarm so that docker containers will run in a distributed environment and it can be easily managed.

Docker run: The docker run command first creates a writeable container layer over the specified image, and then starts it using the specified command.

*Demonstrate steps to lock a swarm cluster<br>
```docker 
docker swarm init --autolock
systemctl restart docker
#Check if locked - the command bellow will return a lock exception
docker service ls
docker swarm unlock
```

```docker
#For update a existing swarm with autolock true or false
docker swarm update --autolock=true

# For generate a new key
docker swarm unlock-key --rotate
```
* Extend the instructions to run individual containers into running services under swarm<br>
```docker 
# Alpine pinging docker.com
 docker service create --replicas=3 --name vinicius-container2 alpine ping docker.com
```
* Interpret the output of docker inspect commands<br>
```docker
#Inspecting a service
docker service inspect vinicius-container --pretty
```
```
Example returns : 
    ID:		2t0m12e1vrglqn7ms43z0jtkt 
    Name:		vinicius-container2 
    Service Mode:	Replicated 
     Replicas:	3
    Placement:
    UpdateConfig:
     Parallelism:	1
     On failure:	pause
     Monitoring Period: 5s
     Max failure ratio: 0
     Update order:      stop-first
    RollbackConfig:
     Parallelism:	1
     On failure:	pause
     Monitoring Period: 5s
     Max failure ratio: 0
     Rollback order:    stop-first
    ContainerSpec:
     Image:		alpine:latest@sha256:72c42ed48c3a2db31b7dafe17d275b634664a708d901ec9fd57b1529280f01fb
     Args:		ping docker.com 
     Init:		false
    Resources:
    Endpoint Mode:	vip
 ```
 
 ```docker
 #Inspecting a container
 docker inspect <id>
 
 ```

* Convert an application deployment into a stack file using a YAML compose file with docker stack deploy<br>
```docker 
#touch docker-stack.yml
docker stack deploy --compose-file docker-stack.yml teste
docker stack ls
docker stack ps teste

#outros comandos
docker stack deploy	#Deploy a new stack or update an existing stack
docker stack ls	#List stacks
docker stack ps	#List the tasks in the stack
docker stack rm	#Remove one or more stacks
docker stack services #List the services in the stack
```
* Manipulate a running stack of services<br>
```docker 
docker stack services --filter <> || name=<> || --format = "{{.Name}}"

```
* Increase number of replicas<br>
```docker 
docker service scale nginx=10
docker service update --replicas=10 nginx

```
* Add networks, publish ports<br>
```docker 
docker network create --driver overlay minha-network
docker service create --replicas=3 --network minha-network --publish 8080:80 nginx
```
* Mount volumes<br>
```docker 
docker volume create my_volume
docker run -d --name mywebserver01 -v my_volume:/var/www/html nginx
docker run -d --name mywebserver02 --mount src=my_volume,dst=/var/www/html nginx
docker rm -f mywebserver01 mywebserver02
```
* Illustrate running a replicated vs global service<br>
```docker 
docker service create --name webteste --replicas 2 nginx
docker service create --name webteste2 --mode global alpine top
docker service ls
docker service ps webteste
docker service ps webteste2
docker node ls

There are two types of service deployments, replicated and global.

For a replicated service, you specify the number of identical tasks you want to run. For example, you decide to deploy an HTTP service with three replicas, each serving the same content.

A global service is a service that runs one task on every node. There is no pre-specified number of tasks. Each time you add a node to the swarm, the orchestrator creates a task and the scheduler assigns the task to the new node. Good candidates for global services are monitoring agents, an anti-virus scanners or other types of containers that you want to run on every node in the swarm.

```
* Identify the steps needed to troubleshoot a service not deploying<br>
```docker 
docker service create \
> -d --name alertmanager \
> -p 9093:9093 \
> --restart-delay 1m \
> --constraint node.role==manager \
> prom/alertmanager \
> --config.file=/path/to/broken.yml

docker service ls
docker service ps alertmanager 
docker inspect <id>
docker logs <id>

```
* Apply node labels to demonstrate placement of tasks<br>
```docker 
docker service create --name my-nginx --replicas 3 nginx
docker node update --label-rm region ubuntu
```
* Sketch how a Dockerized application communicates with legacy systems<br>
```docker 
By default, the container is assigned an IP address for every Docker network it connects to. The IP address is assigned from the pool assigned to the network, so the Docker daemon effectively acts as a DHCP server for each container. Each network also has a default subnet mask and gateway.

When the container starts, it can only be connected to a single network, using --network. However, you can connect a running container to multiple networks using docker network connect. When you start a container using the --network flag, you can specify the IP address assigned to the container on that network using the --ip or --ip6 flags.

When you connect an existing container to a different network using docker network connect, you can use the --ip or --ip6 flags on that command to specify the container’s IP address on the additional network.

In the same way, a container’s hostname defaults to be the container’s ID in Docker. You can override the hostname using --hostname. When connecting to an existing network using docker network connect, you can use the --alias flag to specify an additional network alias for the container on that network.

```
* Paraphrase the importance of quorum in a swarm cluster<br>
```docker 
https://docs.docker.com/engine/swarm/admin_guide/#maintain-the-quorum-of-managers
https://docs.docker.com/engine/swarm/admin_guide/#add-manager-nodes-for-fault-tolerance
https://docs.docker.com/engine/swarm/admin_guide/#recover-from-losing-the-quorum
```
* Demonstrate the usage of templates with docker service create<br>
```docker 
 docker service create \
--name hosttempl \
--constraint node.role==manager \
--hostname="{{.Node.Hostname}}-{{.Node.ID}}-{{.Service.Name}}" \
busybox top

```
