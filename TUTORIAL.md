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

