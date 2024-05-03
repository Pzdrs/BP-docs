---
title: Installation
description: Docs intro
---

You have a two main options for deploying the ESGPS dashboard, depending on your needs. The recommended and most well documented option is [Docker](docker.md). With the help of Docker Compose, you can have the dashboard up and running in a matter of minutes. We offer docker images for a majority of platforms. You can also install and run the dashboard from [source](source.md) if Docker is not your thing.

The ES-GPS application consists of two primary components: the back-end, responsible for handling the heavy lifting, and the front-end, which serves as the user interface. A third crucial component is a proxy server tasked with routing requests to the appropriate back-end or front-end based on the domain name. Although not directly integrated into the ES-GPS application, this __proxy server__ is indispensable for the application's proper functionality. Nginx stands out as the recommended choice for this role, and accordingly, we provide the necessary configuration files for its setup.

## Back-end
The back-end is a Java application that uses the Spring Boot framework. It is responsible for handling all the business logic, as well as communicating with the database and the MQTT broker. It also serves as the front-end API.

Java applications are packaged into JAR files, which can be run on any machine, thanks to the JVM (Java Virtual Machine), which makes Java applications platform-independent.

## Front-end

The front-end is a separate web application that uses the back-end's API to read and write data while providing a user-friendly interface. The application is built using the Vue.js framework, which is a popular JavaScript framework for building user interfaces and single-page applications.