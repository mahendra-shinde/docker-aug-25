# Docker Compose and Multi Container Applications

Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services, networks, and volumes, then create and start all the services with a single command.

## Why Use Docker Compose?

- Manage multi-container applications easily.
- Define services, networks, and volumes in a single file.
- Simplifies development, testing, and deployment.

## Basic Demo

Let's create a simple application with a web server (using Nginx) and a Redis database.

### 1. Create a `docker-compose.yml` file

```yaml
version: '3'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
  redis:
    image: redis:alpine
    deploy:
      replicas: 3
```

This file defines two services:
- **web**: Runs Nginx and maps port 8080 on your host to port 80 in the container.
- **redis**: Runs a Redis server.

### 2. Start the Application

Run the following command in the directory containing `docker-compose.yml`:

```sh
docker-compose up -d
```

This command will:
- Pull the required images if not present.
- Create and start the containers as defined.
- Runs in background due to -d switch

### 3. Stop the Application

To stop the running containers, 

```sh
docker-compose down
```

This command stops and removes the containers, networks, and default volumes.

## Common Docker Compose Commands

| Command                      | Description                                                      |
|------------------------------|------------------------------------------------------------------|
| `docker-compose up`          | Build, (re)create, start, and attach to containers for a service.|
| `docker-compose up -d`       | Start containers in detached mode (in the background).           |
| `docker-compose down`        | Stop and remove containers, networks, images, and volumes.       |
| `docker-compose ps`          | List containers managed by Compose.                              |
| `docker-compose logs`        | View output from containers.                                     |
| `docker-compose build`       | Build or rebuild services.                                       |
| `docker-compose stop`        | Stop services without removing them.                             |
| `docker-compose start`       | Start existing stopped services.                                 |

### Explanation

- **up**: Starts all services defined in the file.
- **down**: Stops and removes all resources created by `up`.
- **-d**: Runs containers in the background.
- **ps**: Shows the status of running containers.
- **logs**: Displays logs from all services.

## Summary

Docker Compose makes it easy to manage multi-container applications with simple commands and a single configuration file.

