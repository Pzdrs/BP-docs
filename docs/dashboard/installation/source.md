---
title: Source Installation
description: Install and run homepage from source
---


!!! danger "Aimed at experienced users"

    As installation from source is way more involved than Docker installation, there are presumptions about the readers' experience, thus not every single step is documented.

Of course, as the entire code base of the application is open-source and available on GitHub, installation from source is an option for experienced users who want special control over the deployment.

Similarly to the previous section about Docker installation, the repository needs to be cloned first:

```bash
git clone https://github.com/Pzdrs/BP-app.git
cd BP-app
```

## Back-end

Starting with the back-end, the Java application needs to be packaged into a JAR file first, before it can be run. The project uses **Gradle** and the wrapper is included in the repository, which makes this step very straightforward.

```bash
cd backend
gradle build -x test # skipping tests, as there are none ;)
```

This packages the application into a JAR file and places it into the `build/libs` directory. Now that we have a JAR file, we can start the application with the following command (you might want to move the file to a more permanent location first):

```bash
java -jar build/libs/backend-0.0.1-SNAPSHOT.jar
```

## Front-end

The front-end part of the application is somewhat less involved. The first step is to install the dependencies listed in `package.json`.

```bash
npm install
```

After all dependencies are installed comes thet build process by running the appropriate `npm` script, which uses the Vite build lifecycle.

```bash
npm build
```

This builds the application and places the output files in the `dist` directory.

## Nginx

Nginx can be installed in multiple different [ways](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/). The simplest way is to use a package manager (`apt` in this case):

```bash
apt install nginx
```

The front-end build files should at this stage be moved to the `/usr/share/nginx/html` directory.

The necessary configuration is included in the `nginx` directory of the cloned repository (`default.conf`).

Running Nginx is now as simple as:

```bash
nginx -g daemon off;
```
