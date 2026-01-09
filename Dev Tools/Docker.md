Docker uses [[iptables]] for request routing. One of its features - it controls routing within the whole docker internal network, so if you do port-forwarding in `container-1`, all containers that request forwarded port of `container-1` over the `iptables` will get to the forwarding target.
### Dockerfile

`RUN` – it is an image build step, the state of the container after a *RUN* command will be committed to the container image. A Dockerfile can have many *RUN* steps that arrange new layers on top of the others to build the image.

---
`ENTRYPOINT` – specifies a command that always runs when the container starts. It can be overridden only if you add a special `--entrypoint` argument running the image.
`docker run --entrypoint <ep> <image> <cmd>`
```
FROM ubuntu
ENTRYPOINT ["echo"]
```
`docker run myimage Hello` -> `echo "Hello"`
`docker run --entrypoint ls myimage -l` -> `ls -l`

---
`CMD` - it is the command that the container executes by default when the container starts. It specifies arguments that will be fed to the *ENTRYPOINT*. You can override the default *CMD* specifying a custom command after the image name.
`docker run <image> <cmd_arg_1> <cmd_arg_2>`
```
FROM ubuntu
CMD ["echo", "Hello, World!"]
```
`docker run myimage` -> `echo "Hello, World!"`
`docker run myimage ls -l` -> `ls -l`

---
`ADD` and `COPY`
- `COPY` just copies everything
- `ADD` allows the source to be an URL
- `ADD` considers `tar` archives as regular directories and works with them as they were just regular directories.

---
`ARG` and `ENV`
- `ENV` – environment variable, available from build image step to container runtime step. This is a plain old environment variable for an application. 
- `ARG` – argument, that can be specified in docker build command. Exists only in Dockerfile and build steps.

---
**Shell and Exec Forms**
- **Exec form**: `ENTRYPOINT ["echo", "ping"]` (preferred).
	- The command runs directly with `PID 1` without `/bin/sh` call.
	- Running process receives signals like `SIGTERM` correctly.
	- `ENTRYPOINT ["echo"]`+`CMD ["ping"]`→ command is `echo ping`.
	- Does not see `ENV`: `ENTRYPOINT ["echo", "$MY_VAR"]`→ output is "*$MY_VAR*".
- **Shell form**: `ENTRYPOINT echo ping`.
	- Docker wraps command into a shell: `/bin/sh -c "echo ping"`.
	- `PID 1` is *shell* (`/bin/sh`). Application is a child process and has another `PID`
	- Running process can't receive `SIGTERM` and will be killed forcibly.
	- `ENTRYPOINT ["echo"]`+`CMD ping`→ command is `echo /bin/sh -c "ping"`.
	- Does see `ENV`: `ENTRYPOINT echo $MY_VAR`→ output is "*VAR_VALUE*".
	- `ENTRYPOINT echo`+ any`CMD...`→ all args from `CMD` or command line are just ignored.

If you want to accept `CMD` and use env vars either, you can run shell explicitly:
- `ENTRYPOINT ["/bin/sh", "-c", "echo $MY_VAR"]`
This scenario has a flaw: app is a child process, has `PID` other than `1` and doesn't receive `SIGTERM`. If you wanna fix it, you have to upgrade this command like below:
- `ENTRYPOINT ["/bin/sh", "-c", "exec echo $MY_VAR"]`
But this scenario has a flaw too: it can't use `CMD` arguments. To fix it, use the following pattern:
- `ENTRYPOINT ["/bin/sh", "-c", "exec echo $MY_VAR \"$@\"", "--"]`
	- `\"$@\"` tells *shell* to place all args (that come from the `CMD`) here.
	- `--` takes place of `$0` parameter (*script name* in `POSIX`) so the `CMD` args start with `$1`

[entrypoints](https://stackoverflow.com/a/34245657) and [about Dockerfile on Habr](https://habr.com/ru/company/ruvds/blog/439980/)

### Misc

To access localhost from docker container specify `host.docker.internal` instead of `localhost`