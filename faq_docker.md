

# Content

TBA




# Basics


## start

### fire up & running:
```
$ docker-compose  up
```

### providing config:
```
$ docker-compose  -f /path/to/docker.yml  run  --rm  builder
```


## configs

The simpliest config set possible to getting started:
```
$ cat env.Dockerfile

# Distro which should be available on docker hub
FROM ubuntu:24.04

# User name
LABEL maintainer="name here"

# Default current dir when container starts
WORKDIR /build/data

# List of packages to be preinstalled before launching the container
ARG APT_PACKAGES="git"

# RUN commands to execute on starting the container:
## update package base
RUN apt update -y
## pre-install packages
RUN apt install -y ${APT_PACKAGES}

# Git trust to avoid related warning - uncomment the next line if git actions & commands are required
#RUN git config --global --add safe.directory /build/data

# Copy the whole current working dir into container
COPY  .  /build/data
```


## delete

### clean build cache objects:
```
$ docker  system  prune  --filter label=CONTAINER_LABEL_NAME  --force
```

### remove image container(s):
```
$ docker  rmi  CONTAINER_LABEL_NAME
```


