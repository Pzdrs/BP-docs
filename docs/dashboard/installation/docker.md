---
title: Docker Installation
description: Install and run the dashboard with Docker
---


Containerization is not a new concept, but it has gained a lot of traction in recent years. Docker is one of the most popular containerization tools, and it is used to package applications and their dependencies into isolated containers. This allows for easy deployment and scaling of applications, as well as ensuring that the application runs the same way on different environments. LXC (Linux Containers) is another popular containerization tool, but Docker, which is built on top of LXC, is more user-friendly and has a larger community.

All Docker images for the ES-GPS dashboard are hosted on [Docker Hub](https://hub.docker.com/r/esgps/dashboard). 

## Prerequisites

1) [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on your machine.

2) A working internet connection to pull the necessary Docker images.

## Installation

1) Clone the ES-GPS repository:

```bash
git clone
```

2) Navigate to the `docker` directory:

```bash
cd es-gps/docker
```

3) Create a `.env` file in the `docker` directory with the following content:

```bash
# Database
POSTGRES_USER=esgps
POSTGRES_PASSWORD=esgps
POSTGRES_DB=esgps

# MQTT
MQTT_HOST=mosquitto
MQTT_PORT=1883
MQTT_USERNAME=esgps
MQTT_PASSWORD=esgps

# Backend
BACKEND_PORT=8080

# Frontend
FRONTEND_PORT=80
```

4) Run the following command to start the ES-GPS dashboard:

```bash
docker-compose up
```

5) Access the dashboard at `http://localhost`.

## Configuration

The `.env` file contains environment variables that are used to configure the ES-GPS dashboard. You can change the values of these variables to suit your needs. The following is a list of the available environment variables:

