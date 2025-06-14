Docker uses [[iptables]] for request routing. One of its features - it controls routing within the whole docker internal network, so if you do port-forwarding in `container-1`, all containers that request forwarded port of `container-1` over the `iptables` will get to the forwarding target.
### Dockerfile

`RUN` – it is an image build step, the state of the container after a *RUN* command will be committed to the container image. A Dockerfile can have many *RUN* steps that arrange new layers on top of the others to build the image.

`CMD` - it is the command that the container executes by default when the container starts. It specifies arguments that will be fed to the *ENTRYPOINT*. You can override the default *CMD* specifying a custom command after the image name.
`docker run <image> <cmd_arg_1> <cmd_arg_2>`

```
FROM ubuntu
CMD ["echo", "Hello, World!"]
```
`docker run myimage` -> `echo "Hello, World!"`
`docker run myimage ls -l` -> `ls -l`

`ENTRYPOINT` – specifies a command that always runs when the container starts. It can be overridden only if you add a special `--entrypoint` argument running the image.
`docker run --entrypoint <ep> <image> <cmd>`

```
FROM ubuntu
ENTRYPOINT ["echo"]
```
`docker run myimage Hello` -> `echo "Hello"`
`docker run --entrypoint ls myimage -l` -> `ls -l`

---

`ADD` and `COPY`
- `COPY` just copies everything
- `ADD` allows the source to be an URL
- `ADD` considers `tar` archives as regular directories and works with them as they were just regular directories.

---

`ENV` – environment variable, available from build image step to container runtime step. This is a plain old environment variable for an application. 

`ARG` – argument, that can be specified in docker build command. Exists only in Dockerfile and build steps.

[entrypoints](https://stackoverflow.com/a/34245657) and [about Dockerfile on Habr](https://habr.com/ru/company/ruvds/blog/439980/)

### Misc

To access localhost from docker container specify `host.docker.internal` instead of `localhost`