# Volumes

Run container that alters files to image write layer
```
$ docker container run --name demo \
    alpine /bin/sh -c 'echo "test" > sample.txt'
```

See what files were altered with diff
```
$ docker container diff demo
```

## Creating volumes

Create a volume
```
$ docker volume create my-data
```

Determine where volumn is stored on host system
```
$ docker volume inspect my-data
```

## Mounting a volume

Run a container and mount the volume
```
$ docker container run --name test -it \
    -v my-data:/data alpine /bin/sh
```

Create a file and exit
```
$ cd /data
$ echo "some data" > data.txt
$ exit
```

Remove the test container.
Make a new container and mount the previous volume to it. 
The data persists.
```
$ docker container rm test
$ docker container run --name test2 -it \
    -v my-data:/app/data \
    centos:7 /bin/bash
$ cd /app/data
$ cat data.txt
```

## Removing volumes

Removing a volume destroys the data irreversibly

Delete a volume
```
$ docker volume rm my-data
```

## Share data between containers

Create a container with a volume mounted
```
$ docker container run -it --name writer \
    -v shared-data:/data \
    alpine /bin/sh
$ echo "I can create a file" > /data/sample.txt
$ exit
```

Create a container with a read only volume mounted
```
$ docker container run -it --name reader \
    -v shared-data:/app/data:ro \
    ubuntu:17.04 /bin/bash
$ ll /app/data
$ echo "try to break read only" > /app/data/data.txt
$ exit
``` 

Remove all containers and volumes
```
$ docker container rm -f $(docker container ls -aq)
$ docker volume rm $(docker volume ls -q)
```

## Using host volumes

Start alpine container with shell and mount subfolder src of current directory to the container at /app/src. Volumes need to use absolute paths
```
$ docker container run --rm -it \
    -v $(pwd)/src:/app/src \
    alpine:latest /bin/sh
```

### Make nginx server

Create index.html
```
$ echo "hello" > index.html
```

Create Dockerfile
```
FROM nginx:alpine
COPY . /usr/share/nginx/html
```

Build the image
```
$ docker image build -t my-website:1.0
```

Run the container with the current directory mounted
```
$ docker container run -d \
    -v $(pwd):/usr/share/nginx/html \
    -p 80:80 --name my-site \
    my-website:1.0
```

Visit localhost, update index.html, refresh localhost, site content changes.

## Define volumes in images

Pull mongo image
```
$ docker image pull mongo:3.7
```

Inspect the image for volumes. 
```
$ docker image inspect \
    --format='{{json .ContainerConfig.Volumes}}' \
    mongo:3.7 | jq
```

Run an instance of mongo
```
$ docker run --name my-mongo -d mongo:3.7
```

Inspect container for volumes created. The source field is the path to the host directory where the data produced by the container will be stored.
```
$ docker inspect --format '{{json .Mounts}}' my-mongo | jq
```





