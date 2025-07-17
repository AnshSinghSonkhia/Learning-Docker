# Docker Tutorial

## Docker Container

A **Docker container** is a lightweight, standalone, and executable package that includes everything needed to run a piece of software: code, runtime, system tools, libraries, and settings. Containers are isolated from each other and the host system, ensuring that applications run the same regardless of where they are deployed.

**Why use Docker containers?**
- **Consistency:** Ensures your app runs the same in development, testing, and production.
- **Isolation:** Keeps applications and their dependencies separate, avoiding conflicts.
- **Portability:** Containers can run on any system with Docker installed.
- **Efficiency:** Containers are lightweight and use fewer resources than virtual machines.
- **Scalability:** Makes it easy to scale applications up or down as needed.

## Use Case: Running Multiple Node.js Apps with Different Versions

Suppose you have two applications that require different versions of Node.js. With Docker, you can create separate containers for each app, each with its own Node.js version. This ensures compatibility and avoids conflicts on your host machine.

**Example:**
- App 1 runs in a container with Node.js 14.
- App 2 runs in a container with Node.js 18.

Each app is isolated in its own environment, making development and deployment easier.

# Docker Image
## What is a Docker Image?

A **Docker image** is a read-only template that contains the instructions for creating a Docker container. It includes the application code, runtime, libraries, environment variables, and configuration files needed to run the application.

## Docker Container vs Docker Image

### Key Differences

| Feature            | Docker Image                                  | Docker Container                          |
|--------------------|-----------------------------------------------|-------------------------------------------|
| Definition         | Blueprint or template for containers          | Running instance of an image              |
| State              | Static, read-only                             | Dynamic, can be started, stopped, deleted |
| Purpose            | Used to create containers                     | Executes the application                  |
| Persistence        | Does not change after creation                | Can have changes during runtime           |
| Example            | `node:18` image from Docker Hub               | Container running Node.js app             |

**Summary:**  
A Docker image is like a snapshot or recipe, while a Docker container is the live, running environment created from that image.

## Why use Docker Images?

We can create multiple containers with the same docker image

- **Reproducibility:** Images ensure that containers are created with the exact same environment every time.
- **Versioning:** Images can be versioned and tagged, making it easy to roll back or update applications.
- **Distribution:** Images can be shared via registries like Docker Hub, enabling easy collaboration and deployment.
- **Layering:** Images are built in layers, allowing for efficient storage and reuse of common components.

## How to Use a Docker Image

1. **Pull an Image from Docker Hub**

    Use the `docker pull` command to download an image from Docker Hub:

    ```sh
    docker pull node:18
    ```

2. **Run a Container from an Image**

    Start a container using the downloaded image:

    ```sh
    docker run -it --name my-node-app node:18
    ```

    - `-it` runs the container interactively.
    - `--name` assigns a name to your container.

3. **Build Your Own Image**

    Create a `Dockerfile` in your project directory and build an image:

    ```sh
    docker build -t my-custom-image .
    ```

4. **List Images**

    View all images on your system:

    ```sh
    docker images
    ```

5. **Remove an Image**

    Delete an image you no longer need:

    ```sh
    docker rmi my-custom-image
    ```

These steps help you manage and use Docker images efficiently in your workflow.

# Flags used with docker in CMDs

- `-it`: Interactive Mode
- `-a`: all 
- `-f`: file 
- `-d`: Detach Mode - Runs container in background
- `-e`: used for declaring environment variables
- `--name`: Used to give a custom name to container

# Docker Commands

- `docker pull IMAGE_NAME`  - pull any publicly available docker image.
- `docker images` - get list of all downloaded images.
- `docker run IMAGE_NAME` - Create a container from any image.
- `docker run -it IMAGE_NAME` - Run it in interactive mode.
- `ls` - to get list of files & folders inside that docker container, which is running in interactive mode.
- `mkdir NEW_FOLDER_NAME` - Create new folder inside docker container.
- `exit` - Stop & Exit from the docker container.
- `docker ps -a` - Get list of "all" containers.
- `docker ps` - Get list of "running" containers.
- `docker start CONTAINER_ID_OR_NAME` - start a particular container.
- `docker stop CONTAINER_ID_OR_NAME` - stop a particular container.
- `docker rm CONTAINER_NAME` - remove a particular container.
- `docker rmi Image_NAME` - remove a particular image.

# Pull an specific version

```bash
docker pull IMAGE_NAME:version
```

```bash
docker pull mysql:8.0
```

# Layers of a Docker Image

Docker images are built in layers, each representing a set of changes or additions. These layers stack on top of each other to form the final image used to create containers.

- **Base Layer:** The foundational layer, often an operating system like Ubuntu or Alpine.
- **Layer 1, Layer 2, ..., Layer n:** Each subsequent layer adds files, libraries, or configuration changes. These layers are read-only and shared across images to save space.
- **Container Layer:** When you run a container from an image, Docker adds a writable container layer on top. This is the only layer where changes (like file edits or new installations) are allowed during runtime.

> **Note:** All layers except the container layer are read-only. The container layer is writable and unique to each running container.

## What Happens When You Install a Newer Version of an Already Installed Image?

When you pull a newer version of an image that is already present on your system, Docker downloads only the layers that are different or updated. Existing layers that are unchanged are reused from your local cache, making the process efficient.

- **Old Image Remains:** The previous version of the image is not deleted automatically. Both versions coexist unless you manually remove the older one.
- **Tagging:** Each image version is identified by its tag (e.g., `node:18`, `node:20`). You can specify which version to use when running containers.
- **Containers Unaffected:** Containers created from the older image continue to run as before. New containers will use the newer image if specified.
- **Storage:** Multiple versions may consume more disk space, so it's good practice to remove unused images with `docker rmi`.

**Example:**
```sh
docker pull node:20
```
This command installs Node.js version 20 alongside any existing Node.js images (e.g., version 18).

> **Tip:** Use `docker images` to list all installed image versions and manage them as needed.

---

# Port Binding

## Port Binding with the `-p` Flag

The `-p` flag in Docker is used to map a port on your host machine to a port inside the Docker container. This allows you to access services running inside the container from your local machine or network.

**Syntax:**
```sh
docker run -p HOST_PORT:CONTAINER_PORT IMAGE_NAME
```
- `HOST_PORT`: The port on your local machine.
- `CONTAINER_PORT`: The port inside the container that your application listens on.

**Example:**
Suppose you want to run a MySQL container and access its database from your host machine. MySQL typically listens on port `3306` inside the container. To bind it to port `8080` on your host:

```sh
docker run -p 8080:3306 mysql:8.0
```
Now, you can connect to MySQL on `localhost:8080`.

### Example: Running Old & New MySQL Images

You can run multiple MySQL containers with different versions by specifying different ports:

- **MySQL 5.7:**
    ```sh
    docker run -d --name mysql57 -p 5080:3306 mysql:5.7
    ```
    Access MySQL 5.7 at `localhost:5080`.

- **MySQL 8.0:**
    ```sh
    docker run -d --name mysql80 -p 8080:3306 mysql:8.0
    ```
    Access MySQL 8.0 at `localhost:8080`.

This approach allows you to run and test multiple versions of MySQL simultaneously without conflicts.

# Docker TroubleShoot Commands

`docker logs CONTAINER_ID` - to check the logs
`docker exec -it CONTAINER_ID /bin/bash` - runs commands in an already running container by accessing the bash of it.
`docker exec -it CONTAINER_ID /bin/sh` - runs commands in an already running container by accessing the sh of it.

# Docker vs Virtual Machine

| Feature                | Docker Container                          | Virtual Machine                          |
|------------------------|-------------------------------------------|------------------------------------------|
| Isolation              | Process-level, shares host OS kernel      | Full OS-level, separate guest OS         |
| Resource Usage         | Lightweight, minimal overhead             | Heavy, requires more CPU and RAM         |
| Startup Time           | Seconds                                   | Minutes                                  |
| Portability            | Runs anywhere Docker is supported         | Requires compatible hypervisor           |
| Image Size             | Small (MBs to low GBs)                    | Large (GBs)                              |
| Management             | Simple with Docker CLI                    | More complex, needs VM management tools  |
| Use Case               | Microservices, CI/CD, rapid deployment    | Legacy apps, full OS isolation           |

**Summary:**  
Docker containers are faster, more lightweight, and easier to manage than virtual machines, making them ideal for modern application development and deployment. Virtual machines provide stronger isolation and are better suited for running multiple different operating systems on the same hardware.

# Docker Network

```bash
docker network ls

docker network create NETWORK_NAME
```

## Why is Docker Network Needed?

Docker networks enable containers to communicate with each other, the host, and external systems securely and efficiently. By default, containers are isolated, but networking allows you to connect them for multi-container applications, control traffic, and manage access.

**Key reasons:**
- **Inter-container communication:** Allows containers to discover and interact with each other.
- **Isolation:** Segregates traffic between different applications or environments.
- **Security:** Controls which containers can communicate, reducing attack surface.
- **Scalability:** Supports dynamic service discovery and load balancing.

## Use Case: Multi-Tier Web Application

Suppose you have a web application with separate containers for the frontend, backend, and database. Using Docker networks, you can:

- Connect the frontend and backend containers so they can exchange API requests.
- Restrict database access to only the backend container for security.
- Easily scale services by adding more containers to the network.

**Example:**
```sh
docker network create app-network
docker run -d --name backend --network app-network my-backend-image
docker run -d --name frontend --network app-network my-frontend-image
docker run -d --name db --network app-network my-db-image
```
All containers can communicate over `app-network`, enabling a robust, isolated multi-tier architecture.

### Deleting a network

```sh
docker network rm NETWORK_NAME
```

# Docker Compose 

Docker Compose is a powerful tool that allows you to define, configure, and manage multi-container Docker applications. 

- By using a single `.yaml` file, you can specify all the services, networks, and volumes your application needs, making it easy to set up and run complex environments with just one command.

- This is especially useful for development, testing, and deployment of applications that rely on multiple interconnected services, such as web servers, databases, and caches.

## Example Docker Compose YAML File

Below is a sample `docker-compose.yaml` file that defines a simple multi-container application with a Node.js backend and a MongoDB database:

```yaml
version: "3.8"

services:
    backend:
        image: node:18
        container_name: my-backend
        working_dir: /app
        volumes:
            - ./:/app
        ports:
            - "3000:3000"
        command: ["npm", "start"]
        depends_on:
            - mongo

    mongo:
        image: mongo:6
        container_name: my-mongo
        ports:
            - "27017:27017"
        volumes:
            - mongo-data:/data/db
        enviroment:
            MONGO_INITDB_ROOT_USERNAME: admin
            MONGO_INITDB_ROOT_PASSWORD: qwerty

volumes:
    mongo-data:
```

**How to use:**
1. Save this file as `docker-compose.yaml` in your project directory.
2. Run `docker compose up` to start both services.

```bash
docker compose -f FILENAME.yaml up -d

docker compose -f FILENAME.yaml down
```

This setup allows the Node.js app to connect to MongoDB, with persistent storage for the database.

- With `.yaml` file, docker compose automatically create a new common network. We don't have to define any network, like we do in terminal.

# Dockerizing our App

Dockerizing an app means packaging your application and its dependencies into a Docker image so it can run consistently across different environments.

### Steps to Dockerize a Node.js App

1. **Create a `Dockerfile`**

In your project directory, add a file named `Dockerfile`:

```Dockerfile
# Use official Node.js image as base
FROM node:18

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of your app code
COPY . .

# Expose the port your app runs on
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
```

2. **Build the Docker Image**

Run this command in your project directory:

```sh
docker build -t my-node-app:1.0.0 .
```

3. **Run the Container**

Start your app in a container:

```sh
docker run -p 3000:3000 my-node-app
```

- The app will be accessible at `localhost:3000`.

### Benefits

- Consistent environment for development, testing, and production.
- Easy deployment and scaling.
- Simplified dependency management.

> **Tip:** You can use Docker Compose to manage multi-container setups for apps with databases or other services.


## Important Dockerfile Instructions for Dockerization of Our App

- `FROM`: define the base-image (like, node.js for node app).
- `WORKDIR`: define the Working directory.
- `COPY`: Copy data from host to image.
- `RUN`: Run commands. (could be multiple).
- `CMD`: Run commands (could be only one single).
- `EXPOSE`: Expose the code of our image.
- `ENV`: Define the env variables.

# Docker Volumes

Volumes are `persistent` data stores for containers.

Docker volumes are used to persist data generated and used by containers. Unlike the container’s writable layer, data in volumes is not deleted when the container stops or is removed. This makes volumes ideal for storing databases, logs, and other important files.

## Key Points

- **Persistence:** Data in volumes remains intact even if the container is deleted.
- **Sharing:** Volumes can be shared between multiple containers.
- **Isolation:** Volumes are managed by Docker and stored outside the container filesystem.
- **Backup & Restore:** Volumes can be easily backed up or restored.

## How to Use Docker Volumes

1. **Create a Volume**

```sh
docker volume create my-volume
```

2. **Mount a Volume to a Container**

```sh
docker run -d --name my-app -v my-volume:/app/data my-image
```

- `-v my-volume:/app/data` mounts the volume to `/app/data` inside the container.

3. **List Volumes**

```sh
docker volume ls
```

4. **Inspect a Volume**

```sh
docker volume inspect my-volume
```

5. **Remove a Volume**

```sh
docker volume rm my-volume
```

- **Remove unused anonymous docker volumes**

```sh
docker volume prune
```

## Example Use Case

If your app writes user uploads or database files to disk, mounting a volume ensures this data is not lost when the container is recreated or updated.

> **Tip:** Use volumes for any data that should persist beyond the lifecycle of a single container.

# Docker Network Drivers

## Common Docker Network Drivers

Docker provides several network drivers to control how containers communicate:

- **bridge**: The default network driver. Containers on the same bridge network can communicate with each other, but are isolated from containers on other networks and the host unless ports are published.
- **host**: Removes network isolation between the container and the Docker host. The container shares the host’s networking namespace, making it behave as if it’s running directly on the host.
- **none**: Disables all networking for the container. The container has no network interfaces apart from the loopback device.

**Example:**
```sh
docker run --network bridge my-image
docker run --network host my-image
docker run --network none my-image
```

Choose the appropriate driver based on your application’s networking needs.