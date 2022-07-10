## About this Project

### dev-webserver-docker-migration

That project repository name made sense when I thought of it. The basic idea was to demonstrate the migration of a monolith web application environment to modular, distributed, containerised one, using the Docker system.

This project repository contains the build automation configuration files used to build a slightly reconfigured Apache httpd webserver image and run it within a securely architected network of containers.

If you're interested in reading a brief account of my experiences and thoughts during this early phase of Docker adoption, link to [Docker Containerisation Research](https://adebayo10k.github.io/research.html#DockerContainerisationResearch) on my actual GitHub Pages site.

You can also read a step-by-step account of the process by which this image build configuration was tested in the [Docker Containerisation Project](https://adebayo10k.github.io/projects/dockerise-github-site.html) report on the same GitHub Pages site.


By following the instructions in this document, you will be using your Docker build automation tooling, like the docker-compose tool, to run a set of containers. You'll use the stable, trusted official, public DockerHub images as base images.

The [(Optional) Running the pre-baked DockerHub images](#optional-running-the-pre-baked-dockerhub-images) section at the end of this document offers a quick alternative, but less recommended way to get these containers running, by pulling the pre-baked image from the [adebayo10k DockerHub registry domain](https://hub.docker.com/u/adebayo10k).

## Files
- docker-compose.yaml - the main build automation configuration file.
- webserver/Dockerfile - extends the official Apache httpd image.
- data_hub/Dockerfile - extends the official busybox image.
- adebayo10k.github.io/docs/ - GitHub Pages static site content.



## Purpose

This project is a relatively simple implementation of containerisation. However, its' value is to demonstrate the deployment of a securely architected container network. The principle of maintaining host isolation is rigorously maintained.

- The webserver container is configured to log to a shared Data Volume.
- A stopped, nologin-shell container as a dedicated data-only container.
- A transient container for secure access to data-only container log data.


## Requirements

You'll need a Docker environment, consisting of a runtime environment for containers (the Docker Engine), the Docker Client and Server. Installation and set up of such an environment explained in the [Docker docs](https://docs.docker.com/).

## Installation

With the Docker environment installed on your host, using this project is just a matter of cloning the repository and telling Docker to build, then instantiate some images into containers.

This project includes the separate a10k GitHub Pages repository as a submodule. The clone command must therefore include the `--recurse-submodules` option which will initialise and fetch changes from the submodule repository, like so...

``` bash
git clone --recurse-submodules https://github.com/adebayo10k/dev-webserver-docker-migration.git

```

NOTE: There are around 180Mb of site content source files, so might take a few minute to pull them down.

## Configuration


Depending on whether you've added your user account to the docker group, you may or may not be issuing the `sudo` with each command. If you're interested, you can read my opinion about that in [this short report](https://adebayo10k.github.io/projects/dockerise-github-site.html). I'll include them in this documentation.

## Deployment

In this case, deployment will mean running this distributed, containerised web application within the container runtime environment that Docker has made on your host machine. Just two steps. From within the project build context: 

1. Tell Docker to build the images specified by the build configuration files:

```
sudo docker compose build --no-cache 
```


2. Tell Docker to spin up those images into containers:
```
sudo docker compose up -d --build
```

Output from Docker, something like:

```
[+] Running 4/4
 Network dev-webserver-docker-migration_default Created                                                      0.6s
⠿ Volume "dev-webserver-docker-migration_shared_vol"  Crea...                                                      0.2s
⠿ Container data_only_hub                             Started                                                      5.6s
⠿ Container a10k_site_running                         Started                                                      6.6s
```



### (Optional) Check the container environment

You can now issue some commands to query various aspects of the running containerised environment.

For example, from within project build context, list current project containers:

```
sudo docker compose ps
```
Outputs something like:
```
NAME                COMMAND              SERVICE             STATUS              PORTS
a10k_site_running   "httpd-foreground"   web                 running             0.0.0.0:8072->80/tcp
data_only_hub       "/bin/true"          data_hub            exited (0)
```

For each running container, use the generic `inspect` subcommand with the `--format` option to filter for information about mounts:
```
sudo docker inspect --format='{{json .Mounts}}' a10k_site_running
```
Outputs something like:
```
[{"Type":"volume","Name":"dev-webserver-docker-migration_shared_vol","Source":"/var/lib/docker/volumes/dev-webserver-docker-migration_shared_vol/_data","Destination":"/var/log/data_access_dir","Driver":"local","Mode":"z","RW":true,"Propagation":""}]
```

### Browse the site for a bit

Now you can navigate a browser on your host to `http://localhost:8072`


When the containers are running successfully, you can securely access the log data you're generating, from a transient container. Issue this command to tell it to spin up, report and spin down:

```
sudo docker run --rm --volumes-from data_only_hub busybox:latest ls -lh /var/log/data_access_dir
```

### Container Tear down

Finally, from within the project context, tear everything down:

```bash
sudo docker compose stop && \
sudo docker compose down

sudo docker volume prune
```


## (Optional) Running the pre-baked DockerHub images

If you'd like to just run the pre-built (prototype) webserver and data_hub images from the Docker client. First pull the [public images from DockerHub](https://hub.docker.com/u/adebayo10k), like so:



``` bash
sudo docker pull adebayo10k/dev-webserver-docker-migration_web:latest
sudo docker pull adebayo10k/dev-webserver-docker-migration_data_hub:latest
```

Then run each of these images to spin up their containers:

```bash
sudo docker volume create --name shared_vol

sudo docker run --name data_only_hub -v shared_vol:/log/data_access_dir \
adebayo10k/dev-webserver-docker-migration_data_hub:latest

sudo docker run --name a10k_site_running -v shared_vol:/log/data_access_dir \
adebayo10k/dev-webserver-docker-migration_web:latest
```

... then as above, from [(Optional) Check the container environment](#optional-check-the-container-environment).



## License
No changes or additions. See [LICENCE](./LICENSE).

## Contact

