---
layout: post
title:  "docker compose"
date:   2016-10-04 13:00:00 +0900
categories: docker
---

# install

reffer to:  
[Install Compose](https://docs.docker.com/compose/install/)

# docker-compose.yml

assign image  
image: httpd:lastst  

assign image built by Dockerfile  
build: .  

set command (use bash)  
command: /bin/bash  

write container to hosts file  
{% highlight text %}
links:
- dbserver  
- dbserver:mysql  
{% endhighlight %}

hosts file is written  
{% highlight text %}
192.168.100.10 dbserver
192.168.100.10 mysql
{% endhighlight %}

write container not in same docker-compose.yml to hosts file  
{% highlight text %}
external_links:
- dbserver  
- dbserver:mysql  
{% endhighlight %}

set open port  
{% highlight text %}
ports:
- "3000"  
- "8080:80"  
{% endhighlight %}

set open port only for containers (not for host)  
{% highlight text %}
expose:
- "3000"  
- "8080"  
{% endhighlight %}

mount volume  
{% highlight text %}
volumes:
- /var/log/mysql
- /home/user/mysqllog:/var/log/mysql
- ~/configs:/etc/configs/:ro
{% endhighlight %}

mount all volumes to another container  
{% highlight text %}
volumes_from:
- log
{% endhighlight %}

set environment  
{% highlight text %}
environment:
- bar=foo
{% endhighlight %}

set container name  
container_name: web_app

## commands

create and boot containers  
docker-compose up

create and boot containers in background  
docker-compose up -d

stop one container  
docker-compose stop (container name)

show containers  
docker-compose ps

show logs  
docker-compose logs

implement command  
docker-compose run (container name) /bin/bash

start containers  
docker-compose start

stop containers  
docker-compose stop

force stop containers  
docker-compose kill

remove containers  
docker-compose rm

## prepare wordpress environment

prepare datastore container  

{% highlight text %}
FROM busybox
MAINTAINER 0.1 sample@sample.com
VOLUME /var/lib/mysql
{% endhighlight %}

VOLUME means saving directory  

create dataonly image  
$ docker build -t dataonly .

create dataonly container and start  
$ docker run --name dataonly dataonly

preapre docker-compose.yml  
{% highlight text %}
webserver:
  image: wordpress
  ports:
    - 8080:80
  links:
    - "dbserver:mysql"

dbserver:
  image: mysql
  volumes_from:
    - dataonly
  environment:
    MYSQL_ROOT_PASSWORD: password
{% endhighlight %}

boot  
$ docker-compose up -d

connect mysql  
use another container to connect  
$ docker run -it --link (mysql container):mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'

or  
$ docker exec -it wordpress_dbserver_1 sh -c 'mysql -uroot -p$MYSQL_ROOT_PASSWORD'

dump all databases  
$ docker exec some-mysql sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql

dump one database (wordpress)  
$ docker exec some-mysql sh -c 'exec mysqldump wordpress -uroot -p"$MYSQL_ROOT_PASSWORD"' > dump.dbbackup

# mysql image

docker run command  
$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=password -d mysql

# implement rails commands

$ docker-compose run (app name) rails generate scaffold Article title:string  
$ docker-compose run (app name) rake db:migrate  

## refferences

[library/mysql \- Docker Hub](https://hub.docker.com/_/mysql/)

