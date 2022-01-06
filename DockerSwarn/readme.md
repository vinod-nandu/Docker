---Day 15
=============================================================================
Container Orchestration
==============================
This is the process of handling docker containers running
on multiple linux servers in a distributed environment


Advantages
=================
1 Load Balancing
2 Scalling
3 Rolling update
4 High Availability and Disaster recovery(DR)

LoadBalancing
==================
Each container is capable of sustaining a specific user load
We can increase this capacity by running the same application
on multiple containers(replicas)

Scalling
==============
We should be able to increase or decrease the number of containers
on which our applications are running without the end user
exepriencing any downtime.

Rolling update
=======================
Application running in a live environment should be upgraded or
downgraded to a different version without the end user having any
downtime

Disaster Recovery
======================
In case of network failuers or server crashes still the container
orchestration tools maintain the desired count of containers
and thereby provide the same service to the end user

Popular container orchestration tools
===========================================
1 Docker Swarm
2 Kubernetes
3 OpenShift
4 Mesos


===========================================================================
Setup of Docker Swarm
============================
1 Create 3 AWS ubuntu instances
2 Name them as Manager,Worker1,Worker2
3 Install docker on all of them
4 Change the hostname
  vim /etc/hostname
  Delete the content and replace it with Manager or Worker1 or Worker2
5 Restart
  init 6
6 To initilise the docker swarm
  Connect to Manager AWS instance
  docker swarm init
  This command will create a docker swarm and it will also generate
  a tokenid
7 Copy and paste the token id in Worker1 and Worker2

===============================================================================
Day 16
================================================================================
Load Balancing:
Each docker containers has a capability to sustain a specific
user load.To increase this capability we can increase the 
number of replicas(containers) on which a service can run

UseCase
------------
Create nginx with 5 replicas and check where these replicas are
running

1 Create nginx with 5 replicas
  docker service create --name webserver -p 8888:80 --replicas 5 nginx

2 To check the services running in swarm
  docker service ls

3 To check where these replicas are running
  docker service ps webserver

4 To access the ngonx from browser
  public_ip_of_manager/worker1/worker2:8888

5 To delete the service with all replicas
  docker service rm webserver
=========================================================================
UseCase
===========
Create mysql with 3 replicas and also pass the necessary environment
variables

1 docker service create --name db --replicas 3 
                    -e MYSQL_ROOT_PASSWORD=intelliqit  mysql:5

2 To check if 3 replicas of mysql are running
  docker service ps db

=======================================================================
Scalling
============
This is the process of increasing the number of replicas or decreasing
the replicas count based on requirement without the end user experiencing
any down time.

UseCase
============
Create tomcat with 4 replicas and scale it to 8 and scale it
down to 2 

1 Create tomcat with 4 replicas
  docker service create --name appserver -p 9090:8080 --replicas 4 tomcat

2 Check if 4 replicas are running
  docker service ps appserver

3 Increase the replicas count to 8
  docker service scale appserver=8

4 Check if 8 replicas are running
  docker service ps appserver

5 Decrese the replicas count to 2
  docker service scale appserver=2

6 Check if 2 replicas are running
  docker service ps appserver


=======================================================================
Day 16
=======================================================================
=======================================================================
Rolling updates
======================
Services running in docker swarm should be updated from once
version to other without the end user downtime

UseCase
===========
Create redis:3 with 5 replicas and later update it to redis:4
also rollback to redis:3

1 Create redis:3 with 5 replicas
  docker service create --name myredis --replicas 5 redis:3

2 Check if all 5 replicas of redis:3 are running
  docker service ps myredis

3 Perfrom a rolling update from redis:3 to redis:4
  docker service update --image redis:4 myredis

4 Check redis:3 replcias are shut down and in tis palce redis:4 replicas are running
  docker service ps myredis

5 Roll back from redis:4 to redis:3
  docker service update --rollback myredis

6 Check if redis:4 replicas are shut down and in its place redis:3 is running
  docker service ps myredis

================================================================================
To remove a worker from swarm cluster
docker node update --availability drain Worker1

To make this worker rejoin the swarm
docker node update --availability active Worker1

To make worker2 leave the swarm
Connect to worker2 usig git bash
docker swarm leave

To make manager leave the swarm
docker swarm leave --force

To generate the tokenid for a machine to join swarm as worker
docker swarm join-token worker

To generate the tokenid for a machine to join swarm as manager
docker swarm join-token manager

To promote Worker1 as a manager
docker node promote Worker1

To demote "Worker1" back to a worker status
docker node demote Worker1
