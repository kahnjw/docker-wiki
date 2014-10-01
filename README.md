# docker-wiki

## Tips

1. Need to perform docker-y things on the docker host from within a container?

Mount the docker daemon socket on the container.

```sh
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock user/containerName:tag
```