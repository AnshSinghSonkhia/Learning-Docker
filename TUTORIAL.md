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

# Docker Commands

- `docker pull IMAGE_NAME`  - pull any publicly available docker image.
- `docker images` - get list of all downloaded images.
- `docker run IMAGE_NAME` - Create a container from any image.
- `docker run -it IMAGE_NAME` - Run it in interactive mode.
- `ls` - to get list of files & folders inside that docker container, which is running in interactive mode.
- `mkdir NEW_FOLDER_NAME` - Create new folder inside docker container.
- `exit` - Stop & Exit from the docker container.
- `docker ps -a` - Get list of "all" containers.