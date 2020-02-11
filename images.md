# Images

## Interactive Create Image

Start with base image
```
$ docker container run -it --name sample alpine /bin/sh
```

Add ip utilities 
```
$ apk update && apk add iputils
$ exit
```

Check how the container is different from the base image
```
$ docker container diff sample
```

Persist the modifications to the image
```
$ docker container commit sample my-alpine
```

See the image
```
$ docker image ls
```

See the history
```
$ docker image history my-alpine
```

## Dockerfile Create Image
```
FROM python:2.7
RUN mkdir -p /app
WORKDIR /app
COPY ./requirements.txt /app
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
```

### FROM
Every Dockerfile starts with FROM keyword. This defines the base image.

### RUN
Any valid Linux command. If more than one line: use backslash(\) to indicate to the shell that the command continues on the next line.

### COPY and ADD
Copy some files or directories from the host to the image. The Add keyword will unpack TAR files.
```
COPY . /app
ADD sample.tar /app/bin/
ADD http://example.com/sample.txt /data/
```
 - copy all files and folders form the current directory to the /app folder inside the image
 - unpack sample.tar and place it in the /app/bin/ directory
 - copy the remote file sample.txt from example.com and place it in the /data directory in the image

### WORKDIR
Define the working directory or context used when a container is run from the custom image

This will create a file sample.txt in the root directory of the image
```
RUN cd /app/bin
RUN touch sample.txt
```

This will create a file sample.txt in the /app/bin directory of the image
```
WORKDIR /app/bin
RUN touch sample.txt
```

The cd command alone is not persisted across layers of commands.

### CMD and ENTRYPOINT
These are the commands that will execute when the container is started from a custom image

ENTRYPOINT is the command to run as process ID: 0
CMD is the arguments passed to the ENTRYPOINT command
```
FROM alpine:latest
ENTRYPOINT ["ping"]
CMD ["8.8.8.8", "-c", "3"]
```

Alternatively you can use "Shell" form but ping will not have process ID: 0
```
FROM alpine:latest
CMD ["ping", "8.8.8.8", "-c", "3"]
```

## Build an image
Create folder and navigate to it
```
$ mkdir ~/FundamentalsOfDocker
$ cd ~/FundamentalsOfDocker
```

Create a sample folder and navigate to it
```
$ mkdir sample1 && cd sample1
```

Create this Dockerfile
```
FROM centos:7
RUN yum install -y wget
```

Build the image
```
$ docker image build -t my-centos .
```
 - -t tag the image name my-centos
 - . the period tells the command where the Dockerfile is: current directory

## Multistep build

Create a multibuild Dockerfile to reduce the size of the image created.
```
FROM alpine:3.7 AS build
RUN apk update && apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN mkdir bin
RUN gcc hello.c -o bin/hello

FROM alpine:3.7
COPY --from=build /app/bin/hello /app/hello
CMD /app/hello
```

Build the image and see the image size is small
```
$ docker image build -t hello-small-image .
$ docker image ls | grep hello
```

## Best Practices
 - Containers are ment to be ephemeral. Containers are ment to be stopped, destroyed, and a new one built with minimum setup.
 - Dockerfile commands should be arranged to leverage caching. Build layers in order to reduce build time.
 - Keep layers small, the fewer layers there are, the less build time. Use backslash to chain commands in RUN commands.
 - Use .dockerignore to avoid sending unnecessary files to the docker deamon.
 - Avoid installingg unneccessary packages
 - Use multistage builds to keep images size small.