# Containers

Remove all running containers by force
```
$ docker container rm -f $(docker container ls -aq)
```
