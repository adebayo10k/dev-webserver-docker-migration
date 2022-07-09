# dev-webserver-docker-migration

The 'protyple image is on public...  link to DockerHub account repo

This project repository contains the build automation configuration files used to build an httpd webserver image and run containers... my GitHub Pages site ....

This image is one output from a project to migrate a static GitHub Pages website, running in a traditional monolithic development environment to one with loosely coupled, modular, disposable Docker containers.

The image builds on an Apache httpd image with some configuration changes and loads it with a Github Pages static website using the following Dockerfile instructions:

If you'd like a quick read about the process by which this image was ...

If you're interested in reading a brief account of my experiences and thoughts during the migration of this monolith web application development environment to Docker containerised one, 
- Link to research page for this project on actual github pages site.

... from the point of my initial adoption of Docker containerisation through to completion of a first project...

You can read a step-by-step account of the process by which this image was built and tested on this GitHub Pages website.

## Files
- docker-compose.yaml - the main build automation configuration file.
- webserver/Dockerfile - 
- data_hub/Dockerfile - 


## Purpose


## Requirements
None.

## Prerequisites
None.

## Installation

This project includes the separate a10k repository as a submodule. The command to clone therefore includes the option .... which will # to clone, initialise and fetch changes from the submodule repositories:

``` bash
git clone --recurse-submodules https://github.com/adebayo10k/dev-webserver-docker-migration.git

```

## Running the Script


## Configuration

You can either run this 'prototype' image or build it with your local build configuration tools from the GitHub source.


``` bash
docker pull adebayo10k/adebayo10kgithubio_web:latest
docker pull adebayo10k/adebayo10kgithubio_data_hub:latest
```


When the containers are running successfully, you can then access log data from a transient container. Test secure access...

```bash
docker run --rm --volumes-from data_only_hub busybox:latest ls -lh /var/log/data_access_dir
```

## Parameters
None.

## Logging
None.

## License
No changes from base images and content.

## Contact




## Quick Reference



## About this image


## How to use this image





## License
