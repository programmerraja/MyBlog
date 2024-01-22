+++
title = 'Docker Notes'
date  = 2024-01-15T08:19:32.3232+05:30
tags  = ['docker','devops']
draft = false
+++

## Docker Engine

When you install Docker, you get two major components:
- Docker client →is a command-line tool used to interact with the Docker Engine,
- Docker daemon (sometimes called “server” or “engine”)
In a default Linux installation, the client talks to the daemon via a local IPC/Unix socket at `/var/run/docker.sock`

The Docker engine is the core software that runs and manages containers.The Docker Engine is made from many specialized tools that work together to create and run containers APIs, execution driver, runtime (create containers) , shims, containerd `(to manage container lifecycle operations — start | stop | pause | rm.)`

#### What happen when run a cmd

`docker container run --name ctr1 -it alpine:latest sh`

- When you type commands like this into the Docker CLI, the Docker client converts them into the appropriate API payload and POSTs them to the correct API endpoint.

- The API is implemented in the daemon. The **daemon** communicates with **containerd** via a CRUD-style API over gRPC to create container

- containerd cannot actually create containers. It uses **runc** to do that. It converts the required Docker image into an OCI bundle and tells runc to use this to create a new container.**(it forks a new instance of runc for every container it creates.)**

- **How it’s implemented on Linux**
    - dockerd (the Docker daemon)
    - docker-containerd (containerd)
    - docker-containerd-shim (shim)
    - docker-runc (runc)

## Builders
A builder is a BuildKit daemon that you can use to run your builds. BuildKit is the build engine that solves the build steps in a Dockerfile to produce a container image or other artifacts.

Starting from Docker 18.09, BuildKit is included

**BuildKit** is an advanced and modular container image building tool that is part of the Docker ecosystem. It is designed to improve the performance, flexibility, and security of building container images. BuildKit introduces features like **parallelization, caching improvements**, and the ability to use custom frontends, which allows for more efficient and customizable image building processes.

we need to enable it explicitly. You can do this by setting the **`DOCKER_BUILDKIT`** environment variable to **`1`**. `export DOCKER_BUILDKIT=1`

**`DOCKER_BUILDKIT=1 docker build . —no-cache` → this cmd use the docker build kit and pull the layer parallely and faster then normal build. you can see the build output is running parallely**

To create a multi-platform Docker image that can run on Windows, Linux, and macOS, you can use the **`buildx`** command,`**docker buildx build -t your-app-image --platform linux/amd64,windows/amd64,darwin/amd64 .**`

Before buildKit when we building docker image by setting `cwd .` the hole repo will send to docker engine expect docker ingore file but in buildkit the engine will request for the file it needed

In docker desktop default it use builkit.

#### Features in buildKit
1. Mount
2. Custom front end where we can use go language and other supported language to write the docker file

## Images
Images are made up of multiple layers that get stacked on top of each other and represented as a single object.

A Docker image is just a bunch of loosely-connected read-only layers. To inspect the image with the docker image inspect command

`docker image inspect ubuntu:latest`

- Will print all layers in the image

Docker employs a storage driver (snapshotter in newer versions) that is responsible for stacking layers and presenting them as a single unified filesystem. Examples of storage drivers on Linux include **AUFS , overlay2 , devicemapper , btrfs and zfs** . As their names suggest, each one is based on a Linux filesystem or block-device technology, and each has its own unique performance characteristics.

**Sharing image layers:**  If we downloading the image and that has base layer is unbuntu which we already have it will reuse it

**digests of images:** Every time you pull an image, the docker image pull command will include the image’s digest (cryptographic content hash) as part of the return code. You can also view the digests of images in your Docker host’s local repository by adding the --digests flag to the docker image ls command

**distribution hash:** When we pushing and pulling image to hub we send the layer in compressed manner. which cause the image digest to change so we use distribution hash. which is hash value of compressed image

**dangling images:** A dangling image is an image that is no longer tagged, and appears in listings as `none:none` to get `docker image ls --filter dangling=true`

**manifest list:** User may have different architectures, such as Windows, ARM, and s390x. so when we pulling the image with tag we need to download the our architectures suitable image for that we use manifest list

manifest list contain entries for each architecture the image supports with same tag.

When we pull an image, your Docker client makes the relevant calls to the Docker Registry API running on Docker Hub. If a manifest list exists for the image, it will be parsed to see if an entry exists for Linux on ARM. If an ARM entry exists, the manifest for that image is retrieved and parsed for the crypto ID’s of the layers that make up the image. Each layer is then pulled from Docker Hub’s

#### Args

## Containers
A container is the runtime instance of an image. `docker container run image --name`

**Stopping containers gracefully:** Docker container stop sends a **SIGTERM** signal to the PID 1 process inside of the container. As we just said, this gives the process a chance to clean things up and gracefully shut itself down.If it doesn’t exit within 10 seconds, it will receive a **SIGKILL.**

**Self-healing containers with restart policies:** The following restart policies exist
- **always**  Will restart the container once docker is resarting (**systemlctl restart docker)**
- **unless-stopped**
- **on-failed** will restart a container if it exits with a non-zero exit code

Containerizing an App

```docker
# Use an official Node.js runtime as a base image
FROM node:14

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install the application dependencies
RUN npm install

# Copy the application code to the container
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run your application
CMD ["node", "app.js"]
```

- docker build -t app-image . 
- docker run -p 3000:3000 -d app-image

#### Memory & CPU Constraints

- By default it take the host cpu and memory as much it need
- `docker run —-memory 60m my-image:tag` 60Mb limit
- Docker have OOM killer (out of memory killer) which is enabled by default what it do was when ever the container is getting out of memory it wil the process that consume more memory to disable the OOM `docker run -m 128m --oom-kill-disable my-image:tag`
- **Swap:** Swap is an extension of physical memory (RAM) that allows the operating system to use a portion of the disk as if it were additional RAM. `docker run --memory=512m --memory-swap=1g my_container`
- swap space is typically enabled by default. This means that the operating system may use swap space to store less frequently accessed data when the physical memory (RAM) is fully utilized.
- If we set memory and swap same it won't use swap
- `Note:` Swap may downgrad the performance of the host
- `docker run --cpus .5 my-image:tag` can use only 50% of the CPU
- `docker run —-cpuset-cpus 1,2 my-image:tag` allocate the 2nd and 3rd CPU

#### Enviroment Var
- `docker run -e VARIABLE_NAME=variable_value my_container` 
- `docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' container_id` to inspect the environment variables of a running container.
#### Logs
- `docker logs [container_name_or_id]`
-  `docker events —-since ‘5m’`
- `docker logs -f my-container` follow logs
- `docker run -D ` Run in debugging mode

## Networking
Docker networking comprises three major components:

- The Container Network Model (CNM) →is a specification that defines how container runtimes like Docker should provide networking for containers.
- libnetwork →is the library that implements the CNM specifications. It’s written in Go, and implements the core components outlined in the CNM.
- Drivers →drivers are plugins that implement the CNM specifications through **`libnetwork`**

**The Container Network Model (CNM)**

it defines three building blocks

- **Sandboxes** →is an isolated network stack. It includes; Ethernet interfaces, ports, routing tables, and DNS config.
- **Endpoints**→are virtual network interfaces (E.g. veth ). Like normal network interfaces, they’re responsible for making connections. In the case of the CNM, it’s the job of the endpoint to connect a sandbox to a network.
- **Networks** →are a software implementation of an 802.1d bridge (more commonly known as a switch). As such, they group together, and isolate, a collection of endpoints that need to communicate.

### Drivers
| Driver   | Description                                            |
|----------|--------------------------------------------------------|
| `bridge` | The default network driver.                            |
| `host`   | Remove network isolation between the container and the Docker host. |
| `none`   | Completely isolate a container from the host and other containers. |
| `overlay`| Overlay networks connect multiple Docker daemons together. |
| `ipvlan` | IPvlan networks provide full control over both IPv4 and IPv6 addressing. |
| `macvlan`| Assign a MAC address to a container.                     |

By default docker have bridge network(in windows it called NAT) which assign a unique ip address for each container from the range of 172.17.0.0/16

The default “bridge” network, on all Linux-based Docker hosts, maps to an underlying Linux bridge in the kernel called **“docker0”**  `docker network inspect bridge | grep bridge.name `

to check the docker network inspect (`docker network inspect bridge`) the container.

How to make two docker to communicate among them

1. Create a network `docker network create -d nat localnet`
2. Run two container on same network `docker container run --network localnet name`
3. Communicate with docker name in container `ping containername1`

`Note`: The default bridge network on Linux does not support name resolution via the Docker DNS service.

#### bridge Network
we can create a custom bridge using `docker create network name `which will create the new netowork bind with bridge and allocate the new Ip range and if you want the container to run on this network attach with `—network=name`

#### Host Network
It directly bind the container to host and exposed to access publicly `docker run --network host name` we can access this by host IP.

#### MACVLAN
Connect the container interface through to the hosts interface.it requires the host NIC to be in promiscuous mode (isn’t allowed on most public cloud platforms)

## Volumes

If you want your container’s data to stick around (persist), you need to put it on a volume. Volumes are decoupled from containers, meaning you create and manage them separately, and they’re not tied to the lifecycle of any container. Net result, you can delete a container with a volume, and the volume will not be deleted.

`docker volume create myvol`

`docker volume inspect myvol`

```json
[
	{
		"CreatedAt": "2018-01-12T12:12:10Z",
		"Driver": "local",
		"Labels": {},
		"Mountpoint": "/var/lib/docker/volumes/myvol/_data",
		"Name": "myvol",
		"Options": {},
		"Scope": "local"
	}
]

```
- Mountpoint → where the data will be stored

By default, Docker creates new volumes with the built-in local driver. As the name suggests, local volumes are only available to containers on the node they’re created on. Use the -d flag to specify a different driver. Third-party drivers are available as plugins. These can provide advanced storage features, and integrate external storage systems with Docker

#### Types

- **Host volume** → we explicitly tell the host path and container path
- **Anonymous voulme** → we only tell the container path it will automatically mount the data to `/var/lib/docker/volumes/random_hash/data`
- **Named volume** → Create volume using `docker volume create myvol` and attach it when running `docker run -v myvol:path_in_container`


`docker run —name myapp -p 4000:4000 -v full_path_in_current_host:path_in_docker run imagename`  

we can use this to avoid rebuiliding the images for each file change in our code we can directly mount the current code dir to docker container which will help to avoid building again and again. use this only for dev.


## Multistage Build

This will be used to reduce the size of docker image by removing the unwanted things that no need for running the appliaction.let us assume you have program that can be compiled in to bin so we don’t need the things that we used during building the program.

Example : In nodejs typescript code we no need the src code after compilling in to js so we can just copy the necessary file and remove other things.

```yaml
# Stage 1: Build Stage
FROM node:latest AS build

WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Production Stage (new base layer)
#FROM will start a new base layer old will be ingored
FROM nginx:latest

# Copy built files from the build stage to the production image 
#(the above will be considered as seprate image and removed)
COPY --from=build /app/dist /usr/share/nginx/html

# Other configurations, if needed

# Container startup command for the web server (nginx in this case)
CMD ["nginx", "-g", "daemon off;"]
```


`docker image history image name `→ to print all layers and how it was builded


## Security

#### Scanning for vulernablity
- **Trivy:** Lightweight and easy to use.
- **Clair:** Part of the CoreOS project, designed for container scanning.
- **Dagda:** Extensive vulnerability scanner for Docker containers.
- **Anchore:** Provides detailed analysis and policy enforcement.
- `docker scan <image_name>:<tag>` part of Docker Hub and is designed to analyze Docker images for security vulnerabilities.

## Docker Compose
It was a Python tool (v1) that sat on top of Docker, and allowed you to define entire multi-container apps in a single YAML file. now v3 written in GO

```docker
version:3
services:
  web: #name for the container
    build: . 
		#context if we have docker file give the path here and not give image
		image: imagename
		enviroment:
				DB_URL: value
    ports:
      - "8000:5000"
    volumes:
      - .:/code  #(map current dir (.) to the /code dir in container)
    depends_on:
      - redis #will wait for redis to spin up
		restart: always

  redis:
    image: redis

```

****Compose file Structure****
- **`version`**: Specifies the version of the Docker Compose file syntax. It ensures compatibility and defines which features are available. ****
- **`Services`** Describes the containers that make up the application, specifying their configuration, links, and other details.
    1. **`image`**: Specifies the Docker image to use for the service. It can be an official image from a registry or a custom image.
    2. **`build`**: Defines the build context for the service, allowing you to build a custom image from a Dockerfile.
    3. **`ports`**: Maps container ports to host ports, allowing external access to the service.
    4. **`volumes`**: Mounts volumes from the host or other containers into the service, providing persistent storage.
    5. **`environment`**: Sets environment variables for the service, influencing its runtime behavior.
    6. **`depends_on`**: Specifies dependencies between services, ensuring one service starts only after its dependencies are up.
    7. **`networks`**: Connects the service to specific networks, facilitating communication with other services.
    8. **`command`**: Overrides the default command specified in the Docker image, allowing custom command execution.
    9. **`entrypoint`**: Similar to **`command`**, but it specifies the entry point for the container.
    10. **`restart`**: Defines the restart policy for the service, determining how the container behaves after it exits.
- **`networks`** Defines networks that containers can connect to. This allows you to isolate containers or facilitate communication between them.
- **`volumes`**: Declares named volumes that can be mounted into containers. Volumes persist data beyond the lifetime of a container.
- **`configs`**: Specifies configuration files for services. It allows you to manage configuration separately from the Compose file.
- **`secrets`**: Defines secrets that can be used by services. It helps manage sensitive data securely.
- **`extensions`**: Provides a way to extend the Compose file by referencing external Compose files.

**Docker compose CMDs**

1. **`docker-compose up`**: Builds, (re)creates, starts, and attaches to containers as per the configuration defined in the **`docker-compose.yml`** file.
2. **`docker-compose down`**: Stops and removes containers, networks, volumes, and images created by **`docker-compose up`**.
3. **`docker-compose build`**: Builds or rebuilds services specified in the **`docker-compose.yml`** file. it will build all images with prefix with name of directory
4. **`docker-compose ps`**: Lists the running containers associated with the Docker Compose configuration.
5. **`docker-compose logs`**: Displays log output from services. You can specify the service name to view logs for a specific service.
6. **`docker-compose exec`**: Runs a command in a running service container. Useful for executing one-off commands or accessing a shell within a container.
7. **`docker-compose stop`**: Stops running services defined in the **`docker-compose.yml`** file without removing them.
8. **`docker-compose start`**: Starts stopped services defined in the **`docker-compose.yml`** file.
9. **`docker-compose restart`**: Restarts services. This is equivalent to stopping and then starting services.
10. **`docker-compose down -v`**: Stops and removes containers, networks, volumes, and images, including volumes. Useful for a complete cleanup.



## Best practices

#### caching
- Make sure the order is correct such that it will do caching docker will not apply caching after one non caching stage
```yaml
COPY ./app
RUN apt-get install 
RUN apt-get install 
```
- In above example COPY will always change if we make a code change which cause the doker to not use cache for the upcoming layer so move that to last as possible
- Remove the pkg that no need

## Resources
1. [https://medium.com/datamindedbe/how-we-reduced-our-docker-build-times-by-40-afea7b7f5fe7](https://medium.com/datamindedbe/how-we-reduced-our-docker-build-times-by-40-afea7b7f5fe7)
2. https://btholt.github.io/complete-intro-to-containers/

## Tools
- [A Docker + Kubernetes network trouble-shooting swiss-army container](https://github.com/nicolaka/netshoot)