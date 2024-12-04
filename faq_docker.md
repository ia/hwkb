

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

TBA


## delete

### clean build cache objects:
```
$ docker  system  prune  --filter label=CONTAINER_LABEL_NAME  --force
```

### remove image container(s):
```
$ docker  rmi  CONTAINER_LABEL_NAME
```


