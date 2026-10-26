# Docker Cheat Sheet — Beginner to Practical Use

A practical reference for common Docker commands, with examples you can copy and run.

## Table of Contents

1. [Docker Basics](#1-docker-basics)
2. [Check Docker Installation](#2-check-docker-installation)
3. [Images](#3-images)
4. [Containers](#4-containers)
5. [Ports](#5-ports)
6. [Volumes](#6-volumes)
7. [Bind Mounts](#7-bind-mounts)
8. [Write a Dockerfile](#8-write-a-dockerfile)
9. [Build and Run Your Own Image](#9-build-and-run-your-own-image)
10. [Logs and Debugging](#10-logs-and-debugging)
11. [Execute Commands Inside Containers](#11-execute-commands-inside-containers)
12. [Container Networking](#12-container-networking)
13. [Environment Variables](#13-environment-variables)
14. [Docker Compose](#14-docker-compose)
15. [Copy Files](#15-copy-files)
16. [Resource Usage and Inspection](#16-resource-usage-and-inspection)
17. [Cleanup](#17-cleanup)
18. [Common Workflows](#18-common-workflows)
19. [Important Notes](#19-important-notes)

---

## 1. Docker Basics

- **Image:** A template used to create containers.
- **Container:** An isolated environment created from an image.
- **Volume:** Docker-managed storage that can outlive a container.
- **Bind mount:** Connects a specific folder or file on the host to a path in a container.
- **Dockerfile:** Instructions for building an image.
- **Registry:** A service for storing and sharing images, such as Docker Hub.
- **Port mapping:** Makes a container port reachable through a host port.
- **Docker Compose:** Defines and runs an application made of one or more services.

Typical flow:

```text
Dockerfile -> Image -> Container
                    -> Multiple containers
```

## 2. Check Docker Installation

Check Docker version:

```bash
docker --version
docker compose version
```

Check whether the Docker service is running on Linux with systemd:

```bash
sudo systemctl status docker
```

Start Docker:

```bash
sudo systemctl start docker
```

Start Docker automatically at boot:

```bash
sudo systemctl enable docker
```

Run a test container:

```bash
sudo docker run hello-world
```

If your account does not have permission to access Docker, use `sudo` before Docker commands. Adding a user to the `docker` group grants powerful, effectively root-level access; do so only if you understand the security implications.

## 3. Images

An image is the template from which a container is created.

### Download an image

```bash
docker pull ubuntu:24.04
docker pull nginx:latest
docker pull node:24
docker pull mongo:8
```

### List downloaded images

```bash
docker images
docker image ls
```

### View image details

```bash
docker image inspect ubuntu:24.04
```

### Remove an image

```bash
docker rmi ubuntu:24.04
```

If a container uses the image, you may need to remove that container first.

### Search Docker Hub

```bash
docker search nginx
```

### Tag an image

```bash
docker tag my-app:latest my-app:v1
```

### Push an image to a registry

First authenticate and ensure the image is tagged for your account:

```bash
docker login
docker tag my-app:latest YOUR_DOCKERHUB_USERNAME/my-app:v1
docker push YOUR_DOCKERHUB_USERNAME/my-app:v1
```

Replace `YOUR_DOCKERHUB_USERNAME` with your actual Docker Hub username.

## 4. Containers

### Run a container

```bash
docker run --name my-ubuntu ubuntu:24.04
```

### Run interactively

```bash
docker run -it --name ubuntu-shell ubuntu:24.04 bash
```

Exit the shell:

```bash
exit
```

### Run in the background

```bash
docker run -d --name web nginx:latest
```

### List running containers

```bash
docker ps
```

### List all containers, including stopped ones

```bash
docker ps -a
```

### Stop a container

```bash
docker stop web
```

### Start an existing stopped container

```bash
docker start web
```

### Restart a container

```bash
docker restart web
```

### Remove a stopped container

```bash
docker rm web
```

### Stop and remove a container

```bash
docker rm -f web
```

### Run a temporary container

The container is automatically removed when it exits:

```bash
docker run --rm ubuntu:24.04 echo "Hello from Docker"
```

## 5. Ports

Port mapping connects a host port to a container port.

Syntax:

```text
-p HOST_PORT:CONTAINER_PORT
```

Example: run Nginx and access it from the host browser at `http://localhost:8080`:

```bash
docker run -d --name nginx-web -p 8080:80 nginx:latest
```

Check the mapping:

```bash
docker port nginx-web
docker ps
```

Stop and remove it:

```bash
docker rm -f nginx-web
```

For a Node.js app listening on port `3000` inside the container:

```bash
docker run -d --name node-app -p 3000:3000 my-node-app:latest
```

The app must listen on `0.0.0.0` inside the container, not only `127.0.0.1`, to be reachable through the mapped port.

## 6. Volumes

A volume is Docker-managed storage. It is useful for persistent data, especially databases.

### Create a volume

```bash
docker volume create my-data
```

### List volumes

```bash
docker volume ls
```

### Inspect a volume

```bash
docker volume inspect my-data
```

### Use a volume in a container

```bash
docker run -d \
  --name mongo-db \
  -p 27017:27017 \
  -v mongo-data:/data/db \
  mongo:8
```

This command uses a volume named `mongo-data`. Docker creates it automatically if it does not exist.

### Test persistence using a simple container

Create a volume and write a file into it:

```bash
docker volume create demo-data

docker run --rm \
  -v demo-data:/data \
  ubuntu:24.04 \
  bash -c 'echo "Persistent data" > /data/message.txt'
```

Remove the temporary container, then read the file from another container:

```bash
docker run --rm \
  -v demo-data:/data \
  ubuntu:24.04 \
  cat /data/message.txt
```

Expected output:

```text
Persistent data
```

### Remove a volume

```bash
docker volume rm demo-data
```

Only remove volumes when you no longer need their data. Docker normally will not remove a volume that is still in use.

## 7. Bind Mounts

A bind mount connects a specific path on the host machine to a path inside a container. It is especially useful for local development.

### Create a host folder and file

```bash
mkdir -p "$HOME/docker-learning/myapp"
echo "Hello from the host" > "$HOME/docker-learning/myapp/message.txt"
```

### Mount the folder into a container

```bash
docker run --rm -it \
  --mount type=bind,source="$HOME/docker-learning/myapp",target=/app \
  ubuntu:24.04 bash
```

Inside the container, run:

```bash
cat /app/message.txt
echo "Added inside the container" >> /app/message.txt
exit
```

Back on the host, check the same file:

```bash
cat "$HOME/docker-learning/myapp/message.txt"
```

The added line appears because both paths refer to the same underlying file.

### Read-only bind mount

Use `readonly` when the container should not modify the mounted files:

```bash
docker run --rm \
  --mount type=bind,source="$HOME/docker-learning/myapp",target=/app,readonly \
  ubuntu:24.04 \
  cat /app/message.txt
```

**Note:** A bind mount can hide files that already exist at the target path inside the image. Choose the target path carefully.

## 8. Write a Dockerfile

A Dockerfile contains instructions for building an image.

Example for a simple Node.js app:

Create a project folder:

```bash
mkdir -p "$HOME/docker-learning/node-demo"
cd "$HOME/docker-learning/node-demo"
```

Create `app.js`:

```javascript
const http = require("node:http");

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello from Node.js inside Docker!");
});

server.listen(3000, "0.0.0.0", () => {
  console.log("Server listening on port 3000");
});
```

Create `Dockerfile` (exact filename, with no extension):

```dockerfile
FROM node:24
WORKDIR /app
COPY app.js .
EXPOSE 3000
CMD ["node", "app.js"]
```

What the instructions mean:

- `FROM`: Base image.
- `WORKDIR`: Working directory inside the image/container.
- `COPY`: Copies files from the build context into the image.
- `EXPOSE`: Documents the port the application uses; it does not publish the port by itself.
- `CMD`: Default command when a container starts.

Create `.dockerignore` to exclude unnecessary files from the build context:

```text
node_modules
.git
.env
npm-debug.log*
```

Never bake secrets into an image. Supply secrets at runtime using an appropriate secret-management mechanism.

## 9. Build and Run Your Own Image

Run these commands from the folder containing the Dockerfile:

```bash
cd "$HOME/docker-learning/node-demo"
docker build -t my-node-app:latest .
```

The final `.` means “use the current directory as the build context.”

Check the image:

```bash
docker images
```

Run the container:

```bash
docker run -d --name my-node-container -p 3000:3000 my-node-app:latest
```

Test it:

```bash
curl http://localhost:3000
```

Expected output:

```text
Hello from Node.js inside Docker!
```

View the logs:

```bash
docker logs my-node-container
```

After editing `app.js`, rebuild and recreate the container to use the updated image:

```bash
docker rm -f my-node-container
docker build -t my-node-app:latest .
docker run -d --name my-node-container -p 3000:3000 my-node-app:latest
```

## 10. Logs and Debugging

### View logs

```bash
docker logs my-node-container
```

### Follow logs live

```bash
docker logs -f my-node-container
```

### Show recent logs

```bash
docker logs --tail 100 my-node-container
```

### Show timestamps

```bash
docker logs -t my-node-container
```

### Inspect a container

```bash
docker inspect my-node-container
```

### Check exit status and other details

```bash
docker ps -a
```

### View Docker events

```bash
docker events
```

## 11. Execute Commands Inside Containers

### Open a shell in a running Ubuntu container

```bash
docker exec -it CONTAINER_NAME bash
```

Example:

```bash
docker exec -it my-node-container sh
```

Use `sh` if the image does not include Bash.

### Run one command without opening a shell

```bash
docker exec my-node-container node --version
```

`docker exec` requires the container to be running.

## 12. Container Networking

Containers can communicate over a Docker network.

### Create a network

```bash
docker network create app-network
```

### List networks

```bash
docker network ls
```

### Run containers on the same network

```bash
docker run -d --name app-db --network app-network mongo:8
docker run -d --name app-web --network app-network nginx:latest
```

Containers on the same user-defined bridge network can generally reach each other using container names as DNS hostnames.

For example, an application container can connect to MongoDB using:

```text
mongodb://app-db:27017
```

Do not use `localhost` to reach a different container: inside a container, `localhost` refers to that same container.

### Inspect a network

```bash
docker network inspect app-network
```

### Connect an existing container to a network

```bash
docker network connect app-network CONTAINER_NAME
```

### Disconnect a container

```bash
docker network disconnect app-network CONTAINER_NAME
```

### Remove a network

```bash
docker network rm app-network
```

## 13. Environment Variables

Pass an environment variable at runtime:

```bash
docker run --rm -e NODE_ENV=production node:24 node -p 'process.env.NODE_ENV'
```

Use an environment file:

Create `.env`:

```text
APP_ENV=development
APP_PORT=3000
```

Run a container using it:

```bash
docker run --rm --env-file .env node:24 node -e 'console.log(process.env.APP_ENV, process.env.APP_PORT)'
```

Do not commit `.env` files containing secrets to source control.

## 14. Docker Compose

Docker Compose defines multiple services in one YAML file.

Create a file named `compose.yaml`:

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    restart: unless-stopped
```

Start the service in the background:

```bash
docker compose up -d
```

Open `http://localhost:8080` in your browser.

List Compose services:

```bash
docker compose ps
```

Follow service logs:

```bash
docker compose logs -f
```

Stop and remove the containers and network created by Compose:

```bash
docker compose down
```

For services using named volumes, `docker compose down` preserves the volumes by default. The following command also removes Compose-managed volumes and their data, so use it carefully:

```bash
docker compose down --volumes
```

Rebuild and start services after changing a Dockerfile:

```bash
docker compose up -d --build
```

## 15. Copy Files

Copy from host to container:

```bash
docker cp ./message.txt my-node-container:/app/message.txt
```

Copy from container to host:

```bash
docker cp my-node-container:/app/message.txt ./message-from-container.txt
```

The container does not always need to be running for `docker cp`.

## 16. Resource Usage and Inspection

### Live resource usage

```bash
docker stats
```

### Resource usage for one container

```bash
docker stats my-node-container
```

### Show container processes

```bash
docker top my-node-container
```

### Inspect image layers

```bash
docker history my-node-app:latest
```

### Check Docker disk usage

```bash
docker system df
```

## 17. Cleanup

### Remove stopped containers

```bash
docker container prune
```

### Remove unused images

```bash
docker image prune
```

Remove all images not currently used by containers:

```bash
docker image prune -a
```

### Remove unused volumes

```bash
docker volume prune
```

### Remove unused networks

```bash
docker network prune
```

### Remove unused Docker resources

```bash
docker system prune
```

More aggressive cleanup, including unused images:

```bash
docker system prune -a
```

**Warning:** Prune commands remove resources. `docker volume prune` removes unused volumes, potentially including data you intended to keep. Review what you are deleting before confirming.

## 18. Common Workflows

### Workflow A: Run an image

```bash
docker pull nginx:latest
docker run -d --name nginx-web -p 8080:80 nginx:latest
docker ps
docker logs nginx-web
docker rm -f nginx-web
```

### Workflow B: Build your application

```bash
cd /path/to/project
docker build -t my-app:latest .
docker run -d --name my-app-container -p 3000:3000 my-app:latest
docker logs -f my-app-container
```

Replace `/path/to/project` with the actual folder containing the Dockerfile.

### Workflow C: Keep data after container deletion

```bash
docker volume create app-data
docker run -d --name app-container -v app-data:/data IMAGE_NAME
docker rm -f app-container
docker run -d --name app-container-2 -v app-data:/data IMAGE_NAME
```

Replace `IMAGE_NAME` with an image that actually writes its data to `/data`. Database images often require a specific data directory, such as MongoDB's `/data/db`.

### Workflow D: Develop using host files

```bash
docker run --rm -it \
  --mount type=bind,source="$PWD",target=/app \
  -w /app \
  node:24 sh
```

This shares the current host directory with `/app` in the container. Run your project commands inside the shell. Whether changes appear immediately depends on the application's development server and configuration.

## 19. Important Notes

- An image is a template; a container is an instance created from it.
- Removing a container does not normally remove a separately managed named volume.
- A named volume and a bind mount are different: Docker manages the volume's storage, while a bind mount uses a host path you specify.
- `-p HOST:CONTAINER` publishes a port; `EXPOSE` alone does not.
- Containers on the same user-defined network can communicate using container names.
- Rebuilding an image does not automatically replace an already-created container.
- `docker rm -f` removes a container; it does not remove a separately managed named volume.
- Be careful with prune commands and never remove volumes containing important data without a backup.
- Avoid putting passwords, API keys, or other secrets inside Dockerfiles or image layers.

---

# Docker Interview Questions and Answers

## 1. What is Docker?

Docker is a platform for packaging and running applications in isolated containers.

## 2. What is the difference between an image and a container?

An image is a template; a container is an instance created from that image.

## 3. What is the difference between a container and a virtual machine?

Containers share the host OS kernel. Virtual machines run a separate guest operating system on virtualized hardware.

## 4. What is a Dockerfile?

A text file containing instructions used to build a Docker image.

## 5. What is the difference between `docker run` and `docker start`?

`docker run` creates and starts a new container. `docker start` starts an existing stopped container.

## 6. What is the difference between `docker stop` and `docker rm`?

`docker stop` stops a container. `docker rm` removes a stopped container.

## 7. Why do we use Docker volumes?

To persist data independently of a container's writable layer, especially for databases.

## 8. What is the difference between a volume and a bind mount?

Docker manages a volume's storage. A bind mount connects a specific host path to a container path.

## 9. Does deleting a container delete its volume?

A separately managed named volume normally remains after container deletion unless explicitly removed.

## 10. What does `-p 8080:80` mean?

It maps host port `8080` to container port `80`.

## 11. What is the purpose of `EXPOSE`?

It documents the port used by the application inside the container. It does not publish that port automatically.

## 12. What is a Docker network?

A mechanism that allows containers to communicate with each other and, when configured, with external networks.

## 13. How can two containers communicate?

Connect them to the same user-defined network and use the other container's name as its hostname.

## 14. What is Docker Compose?

A tool for defining and running multi-container applications through a YAML configuration file.

## 15. How do you check container logs?

Use `docker logs CONTAINER_NAME`. Add `-f` to follow logs live.

## 16. What is the purpose of `docker exec`?

It runs a command inside an existing running container.

## 17. What is `.dockerignore`?

A file that excludes unnecessary files from the Docker build context.

## 18. What happens when you rebuild an image?

The image is rebuilt, but existing containers continue using the image they were created from. Recreate the container to use the updated image.

## 19. How can you reduce Docker image size?

Use a suitable small base image, exclude unnecessary files with `.dockerignore`, remove unnecessary packages, and use multi-stage builds where appropriate.

## 20. How should you handle secrets in Docker?

Avoid committing secrets or embedding them in images. Use an appropriate secret manager or runtime secret mechanism and restrict access.

---

**Tip:** Practice one section at a time. Start with images and containers, then ports, volumes, bind mounts, Dockerfiles, networking, and Compose.
