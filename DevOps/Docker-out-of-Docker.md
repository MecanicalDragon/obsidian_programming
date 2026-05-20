Instead of running a nested Docker daemon like [[Docker-in-Docker]] does, **DooD** uses the host's Docker daemon by forwarding the socket to the host machine socket.

`docker run -v /var/run/docker.sock:/var/run/docker.sock docker`

With this approach container uses docker client CLI running commands on the host. It is faster than DinD and more secure.
