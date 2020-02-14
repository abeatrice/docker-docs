# Containers

## Running a container

Run a container that prints hello
```
$ docker container run alpine echo "hello"
```
 - docker: Docker CLI
 - container: Context
 - run: Command
 - alpine: Container image
 - echo "hello": process to run in container

Run a container in the background
```
$ docker container run -d --name ping centos \
    ping -c 5 127.0.0.1
```

List containers
```
$ docker container ls -l
```
 - -a get all containers
 - -q get only ids of containers

## Start or Stop container

Start container by name
```
$ docker container start <name>
```

Stop container by name
```
$ docker container stop <name>
```

## Remove containers

Remove containers
```
$ docker container rm <container id or name> 
```

Remove all containers by force
```
$ docker container rm -f $(docker container ls -aq)
```

## Inspect containers
```
$ docker container inspect <container name>
```

## Exec into a running container
```
$ docker container exec -i -t <container name> /bin/sh
```
 - -i run additional process interactively
 - -t provide TTY(terminal emulator) for the command
 - /bin/sh the process to run

## Attach to a running container
```
$ docker container attach <container name>
```

To quit the container without stopping it
```
$ Ctrl+P
$ Ctrl+Q
```

To detach and leave the container running
```
$ Ctrl+C
```

## Retrive container logs
```
$ docker container logs <container name>
```

get the last few entries
```
$ docker container logs --tail 5 <container name>
```
