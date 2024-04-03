---
title: Prerequisites
description: Prerequisites for the dashboard deployment
---

Before even getting to the deployment process, a few prerequisites need to be established. The application mainly relies on a database for storing application and positional data and an MQTT broker from which the application will subscribe data.

## Database
The ES-GPS dashboard used a document-based database MongoDB. Unlike the MQTT server, the connection details must be specified at deploy time, meaning it can't be changed at run time. The connection parameters are passed to the back-end by using [environment variables](https://en.wikipedia.org/wiki/Environment_variable). There are 3 required parameters that must be passed to the back-end in order for it to start:

- `MONGO_HOST` (required) <br>
    hostname or IP of the database
- `MONGO_USERNAME` (required)
- `MONGO_PASSWORD` (required) <br>
    if no password is set, an empty string must be passed

With these three parameters, given the server and the database can reach each other over the network, the backend is able to connect to the database. By default, the port __27017__ is used to establish the connection, which is the default MongoDB port. If, for some reason, a different port needs to be used, the `MONGO_PORT` environment variable can be passed to the back-end with the specific port.

There are a couple of different ways to deploy a MongoDB instance. One way is to use [Mongo Atlas](https://www.mongodb.com/docs/atlas/tutorial/deploy-free-tier-cluster/) to deploy an instance in the cloud (a free tier is available). Another option is to deploy it yourself. It can be downloaded as a package using your favorite package manager, or Docker can be employed, which makes things much easier. Using plain Docker, a Mongo instance can be spun up with the following command.

```shell
$ docker run --name some-mongo -d mongo:tag
```

In case Docker Compose is used, the following service definition is added to the `docker-compose.yaml`.

```yaml
mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
```

It also may be wise to use bind mounts or volumes in order to persist data across container restarts. More information on that topic can be found in the official [documentation](https://docs.docker.com/storage/).

## MQTT server

The MQTT protocol operates on the principle of a broker and end devices. At its core lies a central server, often termed as a "broker" in messaging parlance, which orchestrates data communication among devices. This setup revolves around a publish-subscribe model, wherein publishers—typically data sources like sensors or location tracking devices—transmit data (messages) to the central broker, which manages active subscribers.

Each message is published on a so-called topic, an organizational unit within the MQTT architecture. In case of this project, the data sources publish data on the __esgps/gnss__ topic that is then also subscribed to by the application. When a message arrives at the broker, it is relayed to all active subscribers listening to that specific topic. 

There are many open-source brokers, most notably the following:

- Eclipse Mosquitto
- RabbitMQ
- HiveMQ
- Redis (an in-memory database that can be leveraged to act as a message queue)

During development, both Mosquitto and RabbitMQ were used and are, therefore, the officially supported message brokers of this project.

In case no preexisting message broker is available, the deployment of one is equally as simple as MongoDB in the previous section. Here is an example of how to deploy a RabbitMQ instance using Docker Compose.

```yaml
version: '3.7'

services:
  rabbitmq:
    image: rabbitmq:tag
    hostname: my-rabbit
    container_name: rabbitmq
    restart: unless-stopped
    environment:
      - RABBITMQ_DEFAULT_USER=username
      - RABBITMQ_DEFAULT_PASS=password
    ports:
      - "5672:5672"  # RabbitMQ default port
      - "1883:1883" # MQTT
    command: | 
        "/bin/bash -c \"rabbitmq-plugins enable --offline rabbitmq_mqtt rabbitmq_web_mqtt rabbitmq_amqp1_0; rabbitmq-server\""
```

The details of configuring the MQTT broker at run-time are covered in the [Usage](../usage/index.md) chapter.