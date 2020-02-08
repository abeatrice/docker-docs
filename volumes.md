# Volumes

run container that alters files to image write layer
```
$ docker container run --name demo \
    alpine /bin/sh -c 'echo "test" > sample.txt'
```

see what files were altered with diff
```
$ docker container diff demo
```

