# Docker
Virtualization
===================
This is the process of running multiple OS's parallelly on
a single pice of h/w.
Here we have h/w(bare metal) on top of which we have host os
and on the host os we install an application called as hypervisor
On the hypervisor we can run any no of OS's as guest OS

The disadvantage of this approach is these application running on the
guest OS have to pass through n number of lavers to access the H/W
resources.

Containarization
======================
Here we have bare metal on top of whcih we install the host Os
and on the hsot OS we install an application called as Docker Engine
On the docker engine we can run any application in the form of containers
Docker is a technology for creating thse containers

Docker achive what is commonly called as "process isolation"
ie all the applications(processes) have some dependency on a specific
OS.This dependency is removed by docker and we can run them on any
OS as containers if we have Docker engine installed

These containers pass through less no of layers to access the h/w resources
also organizations need not spend money on purchasing licenses of different
OS's to maintian various applications

Docker can be used at the the stages of S/W development life cycle
Build---->Ship--->Run



===========================================================================
Docker comes in 2 flavours
Docker CE (Community Edition)
Docker EE (Enterprise Edition)

Setup of Docker on Windows 
==============================
1 Download docker desktop from
  https://www.docker.com/products/docker-desktop

2 Install it

3 Once docker is installed we can use Power shell
  to run the docker commands


Day 2
===============================================================
Setup of Docker on an Ubuntu Linux Machine
--------------------------------------------------
1 Create an Ubuntu linux instances on AWS cloud

2 Connect to that instance using gitbash

3 To install docker
  a) Open https://get.docker.com and Copy paste the below two commands
     curl -fsSL https://get.docker.com -o get-docker.sh
     sh get-docker.sh

======================================================================
Docker Components
=======================
Images and Containers
===========================
A Docker image is a combination of bins/libs that are necessary for a
s/w application to work
A Docker container is a running instance of a docker image.Any number of
containers can be created from one docker images


Docker Client:This is the CLI of docker where the user can execute the
docker commands,The docker client accepts these commands and passes them
to a background process called "docker deamon"

Docker deamon: This process accepts the commands coming from the docker client
and routes them to work on docker images or containers or the docker registry

Docker registry: This is the cloud site of docker where docker images are
stored.This is of two types
1 Public Registry( hub.docker.com)
2 Private Registry(Setup on one of our local servers)

===========================================================================
Important docker commands
==============================
Working on docker images
===============================
1 To pull a docker image
  docker pull image_name

2 To search for a docker images
  docker search image_name

3 To upload an image into docker hub
  docker push image_name

4 To see the list of images that are downloaded
  docker images
  or
  docker image ls

5 To get detailed info about a docker image
  docker image inspect image_name/image_id

6 To delete a docker image that is not linked to any container
  docker rmi image_name/image_id

7 To delete a image that is linked to a container
  docker rmi -f image_name/image_id

8 To save the docker image as a tar file
  docker save image_name

9 To untar this tar file and get  image
  docker load tarfile_name

10 To delete all image
   docker system prune -af
   
====================================================================
Day 3
-------------------------------------------------------------------
11 To create a docker image from a dockerfile
   docker build -t image_name .

12 To create an image from a customised container
   docker commit container_id/container_name image_name

Working on docker containers
==================================
13 To see the list of running containers
   docker container ls

14 To see the list of all containers (running and stopped)
   docker ps -a

15 To start a container
   docker start container_id/container_name

16 To stop a container
   docker stop container_id/container_name

17 To restart a container
   docker restart container_id/container_name
   To restart after 10 seconds
   docker restart -t 10 container_id/container_name

18 To delete a stopped container
   docker rm container_id/container_name

19 To delete a running container
   docker rm -f container_id/container_name

20 To stop all running container
   docker stop $(docker ps -aq)

21 To delete all stopped containers
   docker rm $(docker ps -aq)

22 To delete all running and stopped containers
   docker rm -f $(docker ps -aq)

23 To get detailed info about a container
   docker inspect container_id/container_name

24 To see the logs genearated by a container
   docker logs container_id/container_name

25 To create a docker container
   docker run image_name/image_id
   run command options
   ---------------------
   --name:  USed to give a name to the container
   -d: Used to run the container in detached mode
   -it: Used to open interactive terminal in the container
   -e: Used to pass environment varibales to the container
   -v : Used to attach an external device or folder as a volume
   --volume-from: Used to share volume between multiple containers
   -p : Used for port mapping.It will link the container port with
        host port.Eg: -p 8080:80 Here 8080 is host port(external port)
        nad 80 is container port(internal port)
   -P: Used for automatic port mapping where the container port is
       mapped with some host port that id greate than 30000
   --link : Used to create a link between multiple containers to create a
            microservices architecture.
   --network: Used to start a container on a specific network
   -rm : Used to delete a container on exit
   -m: Used to specify the upper limit on the amount of memeory that 
       a container can use
   -c: Used to specify the upper limit on the amout of cpu a container can use
   -ip: Used to asssign an ip to the container

26 To see the ports used by a container
   docker port container_id/container_name

27 To run any process in a container from outside the container
   docker exec -it container_id/container_name process_name
   Eg: To run the bash process in a container
   docker exec -it container_id/container_name bash

28 To come out of a container without exit
   ctrl+p,ctrl+q

29  To go back into a container from where the interactive terminal is running
    docker attach container_id/container_name

30  To see the processes runnign in a container
    docker container container_id/container_name top

Working on docker networks
----------------------------------
31 To see the list of docker networks
   docker network ls

32 To create a docker network
   docker network create --driver network_type network_name

33 To get detailed info about a network
   docker network insepct network_name/network_id

34 To delete a docker network
   docker network rm network_name/network_id

35 To connect a running container to a network
   docker netowork connect network_name/network_id container_name/container_id

36 To disconnect a running container to a network
   docker netowork disconnect network_name/network_id container_name/container_id

Working on docker volumes
============================
37 To see the list of docker  volumes
   docker volume ls

38 To create a docker volume
   docker volume create volume_name

39 To get detailed info about a volume
   docker volume inspect volume_name/volume_id

40 To delete a volume
   docker volume rm volume_name/volume_id

UseCase 1
================
Create an nginx container in detached mode and name it webserver
docker run --name webserver -d -p 9999:80 nginx

To access  ginx from browser
public_ip_of_dockerhost:9999

=======================================================================
UseCase 2
=============
Create a jenkins container and perform automatic port mapping
docker run  --name myjenkins -d -P jenkins/jenkins

To see the port mapping
docker port myjenkins

To access jenkins from browser
public_ip_of_dockerhost:port_from_step2

====================================================================
Create a centos container and open interactive terminal in it
docker run --name c1 -it centos

To come out of the centos container
exit

======================================================================
UseCase 
===============
Create a mysql container and create few tables in it
docker run --name mydb -d -e MYSQL_ROOT_PASSWORD=intelliqit mysql

To open interactive bash shell in the container
docker exec -it mydb bash

To login as mysql root user
mysql -u root -p
Password: intelliqit

To see the list of databases
show databases;

To move into any of the databases
use sys;

Create emp and dept tables
Open https://justinsomnia.org/2009/04/the-emp-and-dept-tables-for-mysql/
Copy the code and paste it

To see the tables
select * from emp;
select * from dept;
============================================================================
Day 4
=============================================================================
Linking of Container
==========================
To create a multi container architecture we have use the following ways
1 --link option
2 Docker compose
3 Docker network
4 Python Scripts
5 Ansible playbooks

UseCase
Create 2 busybooks containers c1 and c2 and link them

1 Create a busybox contaienr and name it c1
  docker run --name c1 -it busybox

2 To come out of the c1 contaienr without exit
  ctrl+p,ctrl+q

3 Create another busybox container c2 and link it with c1 container
  docker run --name c2 -it --link c1:mybusybox  busybox

4 Check if c2 is pinging to c1
  ping c1

UseCase
=============
Setup wordpress and link it with mysql container

1 Create a mysql container
   docker run --name mydb -d -e MYSQL_ROOT_PASSWORD=intelliqit  mysql:5

2 Create a wordpress container and link with the mysql container
  docker run --name mywordpress -d -p 8888:80 --link mydb:mysql wordpress

3 To check if wordpress and mysql containers are running
  docker container ls

4 To access wordpress from a browser
  public_ip_dockerhost:8080

5 To check if wordpress is linked with mysql
  docker inspect mywordpress
  Search for "Links" section

=========================================================
UseCase
=============
Setup CI-CD environment where a Jenkins container is linked with
2 tomcat containers for QAserver and PRodserver

1 Create a jenkins container
  docker run  --name myjenkins -d -p 5050:8080 jenkins/jenkins

2 To access jenkins from browser
  public_ip_dockerhost:5050

3 Create a tomcat container as qaserver and link with jenkins container
  docker run --name qaserver -d -p 6060:8080 --link myjenkins:jenkins tomcat

4 Create another tomcat container as prodserver and link with jenkins
  docker run --name prodserver -d -p 7070:8080 --link myjenkins:jenkins tomcat

5 Check if all 3 containers are running
  docker container ls
========================
============================
Setup LAMP architecture 

1 Create mysql container
  docker run --name mydb -d -e MYSQL_ROOT_PASSWORD=intelliqit mysql

2 Create an apache container and link with mysql container
  docker run --name apache -d -p 9999:80 --link mydb:mysql httpd

3 Create a php container and link with mysql and apache containers
  docker run --name php -d --link mydb:mysql --link apache:httpd php:7.2-apache

4 To check if php container is linked with apache and mysql
  docker inspect php
============================================================================================
Day 5
================================================================================
UseCase
================
Create a testing environment where a selenium hub container is linked
with 2 node containers one with chrome and other with firefox installed

1 Create a selenium hub container
  docker run --name hub -d -p 4444:4444  selenium/hub 

2 Create a container with chrome installed on it
  docker run --name chrome -d -p 5901:5900 --link hub:selenium 
                                            selenium/node-chrome-debug

3 Create another container with firefox installed on it
  docker run --name firefox -d -p 5902:5900 --link hub:selenium 
                                           selenium/node-firefox-debug

4 The above 2 containers are GUI ubuntu containers and we can
  access their GUI using VNC viewer
  a) Install VNC viewer from
     https://www.realvnc.com/en/connect/download/viewer/

  b) Open vnc viewer--->Public ip of docker host:5901 or 5902
     Click on Continue--->Enter password: secret

=========================================================================
Docker compose
=======================
The disadvantage of "link" option is it is depricated and 
the same individual command have to be given multiple times
to setup similar architectures.
To avaoid this we can use docker compose
Docker compose uses yml files to setup the multu container 
architecture and these files can be resused any number of time

Setup of docker compose

1 Download the docker compose s/w
  sudo curl -L "https://github.com/docker/compose/releases/download/1.29.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

2 Give execute permissions on it
  sudo chmod +x /usr/local/bin/docker-compose

3 To check if docker compose is installed or not
  docker-compose --version


Url: https://docs.docker.com/compose/install/
=====================================================================
Day 6
=====================================================================
USeCase
==============
Create a docker compose file to setup a mysql and wordpress 
container and link them

vim docker-compose.yml
---
version: '3.8'

services:
 mydb:
  image: mysql:5
  environment:
   MYSQL_ROOT_PASSWORD: intelliqit

 mywordpress:
  image: wordpress
  ports:
   - 8888:80
  links:
   - mydb:mysql
...

To setup the containers from the above file
docker-compose up -d

To stop all the container of the docker compose file
docker-compose stop

To start the container
docker-compose start

To stop and delete
docker-compose down

========================================================================
UseCase
=================
Create a docker compsoe file to setup the CI-CD environment
where a jenkins container is linked with 2 tomee containers
one for qaserver and other for prodserver

vim docker-compose.yml
---
version: '3.8'

services:
 myjenkins:
  image: jenkins/jenkins
  ports:
   - 5050:8080

 qaserver:
  image: tomee
  ports:
   - 6060:8080
  links:
   - myjenkins:jenkins

 prodserver:
  image: tomee
  ports:
   - 7070:8080
  links:
   - myjenkins:jenkins
...

========================================================================
UseCase
==============
Create a docker compose file to setup the LAMP architecture
vim lamp.yml
---
version: '3.8'

services:
 mydb:
  image: mysql
  environment:
   MYSQL_ROOT_PASSWORD: intelliqit

 apache:
  image: httpd
  ports:
   - 8989:80
  links:
   - mydb:mysql

 php:
  image: php:7.2-apache
  links:
   - mydb:mysql
   - apache:httpd
...

To create containers from the above file
docker-compose -f lamp.yml up -d

To delete the containers
docker-compose -f lamp.yml down

===========================================================================

=========================================================================
Create a docker compose file to setup the selenium
testing environment where a hub container is linked with 2 node
containers

vim selenium.yml
---
version: '3.8'

services:
 hub:
  image: selenium/hub
  ports:
   - 4444:4444
  container_name: hub

 chrome:
  image: selenium/node-chrome-debug
  ports:
   - 5901:5900
  links:
   - hub:selenium
  container_name: chrome

 firefox:
  image: selenium/node-firefox-debug
  ports:
   - 5902:5900
  links:
   - hub:selenium
  container_name: firefox

To create containers from the above file
docker-compose -f selenium.yml up -d

