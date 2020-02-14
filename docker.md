# Docker

## Obtain Docker System Information

Informaton about the Docker client and server the current configuration is using.
```
$ docker version
```

Information about what mode Docker engine is operating in, the storeage driver, ect.
```
$ docker system info
```

## List resource consumption
```
$ docker system df
```

## Prune unused resources
### Containers
Prune containers: containers not in running status
```
$ docker container prune
```

Prune by force: all containers, including exited or created status
```
$ docker container prune -f
```

Prune all containers, even currently running status
```
$ docker container rm -f $(docker container ls -aq)
```

### Images
Prune images: remove orphaned layers
```
$ docker image prune
```

Prune images by force
```
$ docker image prune -f
```

Prune all images that are not currently in use
```
$ docker image prune --force --all
```

### Volumes
```
$ docker volume prune
```

Prune all volumes that have a specific label
```
$ docker volume prune --filter 'label=demo'
```

### Networks
```
$ docker network prune
```

### Everything
prune everything at once: all unused containers, images, volumes, and networks.
```
$ docker system prune
```

## Docker system events

Output docker system events. NOTE: This command is blocking, open an extra terminal window.
```
$ docker system events
```

See outputt of system events.
```
$ docker container run --rm alpine echo "hello"
```

Use format parameter to output type, image, and action
```
$ docker system events --format 'Type={{.Type}} Image={{.Actor.Attributes.image}} Action={{.Action}}'
```
