---
title: Docker Installation
description: Install and run the dashboard with Docker
---


Containerization is not a new concept, but it has gained a lot of traction in recent years. Docker is one of the most popular containerization tools, and it is used to package applications and their dependencies into isolated containers. This allows for easy deployment and scaling of applications, as well as ensuring that the application runs the same way on different environments. LXC (Linux Containers) is another popular containerization tool, but Docker, which is built on top of LXC, is more user-friendly and has a larger community.

As the Docker images are not hosted anywhere publicly available, the repository needs to be cloned and the images built manually. A convenient Docker Compose file configured with the necessary image contexts is available to the user.

## Before you get started

1) [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on your machine.

2) A working internet connection to clone the repository.

## Installation

1) Clone the ES-GPS repository:

```bash
git clone https://github.com/Pzdrs/BP-app.git
```

2) Navigate to the `BP-app` directory:

```bash
cd BP-app
```

3) Docker Compose is expecting Mongo credentials placed in a `.env` file:

```bash
MONGO_USERNAME=user
MONGO_PASSWORD=password
```

!!! warning "Common pitfall"

    Make sure the `.env` file is located in the same directory as the `docker-compose.yaml`.

4) Run the following command to start the ES-GPS dashboard:

```bash
docker-compose up --build
```

5) As the dashboard is expecting to be accessed at `http://esgps.pycrs.cz`, a DNS record needs to be added to the zone file for the domain *pycrs.cz*:

If the container is deployed with the Macvlan or IPvlan network, thus it has its own IP address:

```
esgps     IN      A     10.0.0.15 # replace with the actual IP address
```

If placed behind a reverse proxy (recommended):

```
traefik   IN      A     10.0.0.25 # example

esgps     IN      CNAME     traefik
```

6) The dashboard is now up and running and depending on the DNS configuration, accessible at `https://esgps.pycrs.cz`.