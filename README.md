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

### Optimize a Nodejs docker build

Docker can be really slow if you are not caching node_modules and run
`npm install` on every build. Instead follow [this](http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/) guide.

Add this to your Dockerfile before after system dependencies, but before the application code is added.

```sh
ADD package.json /tmp/package.json
RUN cd /tmp && npm install
RUN mkdir -p /opt/app && cp -a /tmp/node_modules /opt/app/
```
