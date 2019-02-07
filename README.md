# docker-compose

Docker documentation for the first time (mac install)

Get started : https://docs.docker.com/get-started/

This setup is itended for local development machine with multi domain via Nginx vhost.

## This docker starter included :
* PHP-FPM (php 7.1)
* NGINX (Web Server)
* MYSQL (mariadb:10.1)
* POSTGREE (postgres:10.4)
* PORTAINER (Docker Container Management)
* REDIS (redis:3.2.11)
* MONGODB (mongo:3.6)
* RABBITMQ (rabbitmq:3.7.4-management)
* ADMINER (Database Management)
* SWAGGER EDITOR (API Documentation)

## Install docker for mac :

Below is step by step to install docker
* Install docker apps at : https://docs.docker.com/docker-for-mac/install/
* Clone this github repo on your local computer folder (ex. /docker)
* Setting or edit your file ```Nginx, docker-compose.yml, Dockerfile, /etc/hosts, etc``` (see instruction below)
* Create Network via docker command (see instruction below)
* Run this on clone docker folder ```$docker-compose up --build``` to build & download container images (make sure it downloaded & run smoothly without error)
* Open your browser to test your starter included (portainer, swagger-editor, rabbitmq management, adminer, etc)

## How To Run

```
# $docker-compose up --build (to build all container & download images - for the FIRST TIME, so be patient n make some coffe.. :D)
# $docker-compose up -d (docker run command in daemon proccess)
```

## Docker command reference

```
# $docker-compose build [service.name] - only build single service
# $docker restart $(docker ps -q) - restart/stop all running container process (-q quiet | -a all mode)
# $docker ps -a - (get all process list)
# $docker inspect [service.name] - inspect container IP address & detail build
# $docker exec -it [service.name/container_id] sh - (bash prompt to get access into images/container via SSH terminal)
# $docker volume ls - (get volume space list consume)
# $docker images - (get list of docker image)
# $docker info - (get info docker image/container build)
# $docker stats - (memory consume docker, CPU consume, I/O, Block I/O)
```

## Nginx

Every app need a container for it self (i.e web_1 & web_2) and before run docker please to make sure to map it in different port (i.e web_1 on 8080, web_2 on 8081) to avoid port conflict. Default vhost configuration for localhost are setup in ```./nginx/sites-enabled``` please add/change it for new web/project container.

### Multiple Vhost (ip.address mapping /etc/hosts)

In multiple vhost, there is needed local domain name and IP mapping on to access it, don't forget to setup your ```/etc/hosts``` file.

```
Example :
0.0.0.0		portainer.local
0.0.0.0     mysql.local
0.0.0.0     mongo.local
0.0.0.0     redis.local
0.0.0.0     rabbitmq.local
0.0.0.0     postgree.local
0.0.0.0     adminer.local
0.0.0.0     swagger-editor.local
```

## Create Network and Support Stack

Before this container can be accessible from your container, you need to create a network name ```my-shared-network``` or with other name which you must define it on ```docker-compose.yml``` (ex. localnetwork)

Create docker network with this command :

```
# $docker network create my-shared-network
```

## How To Add More Web App Project (multiple vHost)

I'm using vhost domain to separate webapp. You can add more webapp based on PHP with this step :
* Create a new folder web inside ```app``` for your webapp. i.e ```app/my_webapp```.
* Setup your webapp nginx inside ```nginx/sites-enabled``` with full vhost subdomain and .conf as file extension. i.e ```nginx/sites-enabled/my_webapp.local.conf```.
* Setup other configuration related in your webapp.
* Set your container's domain via ```hostname``` in docker-compose.yml.
* Up and rebuild (if necessary) docker with ```$docker-compose up --build```. Makesure this step is success and docker run smoothly.
* Set your ```hosts file``` and add my_webapp.local domain into it, so your host OS can access it. i.e ```0.0.0.0	my_webapp.local```

## MySQL (MariaDB 10.1)

MySQL persistence data will be saved into ./data/mysql within this root project, so it will still available if you're running docker-compose down.

```
Host : 0.0.0.0:3306
User : root
Pass : {set in MYSQL_ROOT_PASSWORD in docker-compose}
```

## MongoDB (mongo:3.6)

Same with MySQL, data are persistence and will be saved in ./data/mongodb.

```
Host : 0.0.0.0:27017
User : None
Pass : None
```

## Redis (redis:3.2.11)

```
Host : 0.0.0.0:6379
User : None
Pass : None
```

## RabbitMQ (rabbitmq:3.7.4-management)

Installed with management plugin.

```
Web Management Admin : 0.0.0.0:15672 (need user & pass login)
Service Host : 0.0.0.0:5672
User : guest
Pass : guest
```

You can access rabbitmq admin with domain name http://rabbitmq.local:15672/ after you add an entries into your host OS ```/etc/hosts```.

0.0.0.0	rabbitmq.local

## PostgreSQL (postgres:10.4)

You can set your user on environment variable POSTGRES_USER & POSTGRES_PASSWORD on ```docker-compose.yml``` file.

Host : 0.0.0.0:5432

## Adminer (Database Management)

Host : 0.0.0.0:8090

You can access it with this domain http://adminer.local:8090/ after you add an entries into your host OS ```/etc/hosts```.

0.0.0.0	adminer.local

## Swagger Editor (API Documentation)

Host : 0.0.0.0:8091

You can access it with its domain http://swagger-editor.local:8091/ after you add an entries into your host OS ```/etc/hosts```.

0.0.0.0	swagger-editor.local

## Portainer (Docker Container Management)

Host : 0.0.0.0:9100

You can access it with its domain http://portainer.local:9100/ after you add an entries into your host OS ```/etc/hosts```.

0.0.0.0	portainer.local


## NOTE
* everytime shutdown/stop docker container, IP Address container might be changed so you need to edit your ```/etc/hosts``` file before you can access it (to check ip.address container use command ```$docker inspect [container_name]```).

