---
layout: post
title: "Redis setup on Docker"
description: "How to install Redis on Docker"
category: cache software
tags: redis docker cache
---

### Redis setup on Docker

Pull Redis image from Docker Hub:

> docker pull redis

Use this command if you want to use your own Redis configuration file:

> docker run -v `<`/path/to/custom/config/folder/redis.conf`>`:/usr/local/etc/redis/redis.conf --name `<`redis-instance-name`>` redis redis-server /usr/local/etc/redis/redis.conf


Start a Redis instance:

> docker run --name `<`redis-instance-name`>` -d redis


Start Redis instance with persistant storage:

> docker run -v `<`/path/to/persistent/folder`>`:/data --name `<`redis-instance-name`>` -d redis redis-server


If you want to change the port, use this command with above commands:

> -p `<`public-port`>`:6379