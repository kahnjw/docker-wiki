# docker-wiki

## Tips

### Need to perform docker-y things on the docker host from within a container?

Mount the docker daemon socket on the container.

```sh
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock user/containerName:tag
```

### Need to hack around on a container?

Use nsenter:

```sh
$ docker run -v /usr/local/bin:/target jpetazzo/nsenter
$ PID=$(docker inspect --format {{.State.Pid}} <container_name_or_ID>)
$ nsenter --target $PID --mount --uts --ipc --net --pid
```
