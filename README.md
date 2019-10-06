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
docker ps

```
*Demonstrate steps to lock a swarm cluster<br>
```docker 
docker ps

```
* Extend the instructions to run individual containers into running services under swarm<br>
```docker 
docker ps

```
* Interpret the output of docker inspect commands<br>
```docker 
docker ps

```
* Convert an application deployment into a stack file using a YAML compose file with docker stack deploy<br>
```docker 
docker ps

```
* Manipulate a running stack of services<br>
```docker 
docker ps

```
* Increase number of replicas<br>
```docker 
docker ps

```
* Add networks, publish ports<br>
```docker 
docker ps

```
* Mount volumes<br>
```docker 
docker ps

```
* Illustrate running a replicated vs global service<br>
```docker 
docker ps

```
* Identify the steps needed to troubleshoot a service not deploying<br>
```docker 
docker ps

```
* Apply node labels to demonstrate placement of tasks<br>
```docker 
docker ps

```
* Sketch how a Dockerized application communicates with legacy systems<br>
```docker 
docker ps

```
* Paraphrase the importance of quorum in a swarm cluster<br>
```docker 
docker ps

```
* Demonstrate the usage of templates with docker service create<br>
```docker 
docker ps

```
