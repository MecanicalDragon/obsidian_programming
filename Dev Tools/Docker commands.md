**Basic commands**
`export http_proxy="http://spbsrv-proxy1.t-systems.ru:3128"` - export proxy inside ubuntu container
`Ctrl+p ctrl+q` - detach from container
`exit` - exit ubuntu

`docker run hello-world` - run hello-world container
`docker run -it --name ubOs --hostname ubHost ubuntu bash` - run ubuntu, init 'bash' shell
`docker ps` - show working containers
`docker ps -a` - show containers for all time
`docker images` - show used images
`docker images -a` - show used images for all time
`docker start {container name}` - start once launched container
`docker stop {container name}` - stop launched container
`docker attach {container name}` - attach to launched container
`docker diff {cont-name}` - show changed files in container
`docker logs {cont-name}` - show logs of container
`docker rename {cont-name} {new-name}` - no comments
`docker exec {container-name} {inner-command}` - execute command from outer container
`docker export -o {drive:/path/file.tar} {container-name}` - export container file system to tar archive on pc
`docker create [options] {image}` - create container with specified options
`docker attach [options] {image}` - attach local standard input, output, and error streams to a running container

**Removal**
`docker rm {cont-name}` - remove container
`docker rmi {image-name}` - remove image
`docker rm $(docker ps -a -q)` - remove all containers
`docker rmi $(docker images -a -q)` - remove all images
`docker rm -f {container}` - forced remove {container}
`docker rmi -f $(docker images -q)` - forced remove for all images

**Copying**
`docker cp foo.txt mycontainer:/foo.txt` - copy file from pc to container
`docker cp mycontainer:/foo.txt foo.txt` - copy file from container to pc
`docker cp src/. mycontainer:/target` - copy directory from pc to container
`docker cp mycontainer:/src/. target` - copy directory from container to pc

**Images**
`docker build -f Dockerfile -t {container} .` - build {container} with Dockerfile from current directory
`docker commit {name} {login}/{image}:{version}` - create a new image from changed {name} container
`docker push {login}/{image}` - push image to docker cloud repository
`docker pull {login}/{image}` - pull image from docker cloud repository
`docker save -o {filename}/{image_name}` - save image to disk
`docker load -i {filename}` - load image from disk

**Flags**
`docker run -it {container}` - interactive launch (allows to type in commands inside container)
`docker run -d {container}` - launch in background mode (detached)
`docker run --name {container}` - give a name to launching container
`docker run --hostname {container}` - give a hostname to launching container
`docker run -p 8780:8780 {container}` - run {container} with port mapping [pc:docker]
`docker run --rm {container}` - run {container}, remove it, when it'll be stopped
`docker run -v {d:/path}:{inner/path}` - synchronized folder between container and pc
`docker run -e VAR=VAL -e VAR2=VAL2` - set environment values (use with '$')

**Network**
`docker network create --driver bridge {name}` - create bridge network with specified name
`docker network create {name}` - create network with specified name (bridge by default)
`docker network inspect {name}` - show info about network
`docker network connect {network} {container}` - connect launched container to existing network
`docker network disconnect {network} {container}` - disconnect launched container from network
`docker network rm {name} {name2} {name3}` - remove specified networks
`docker network prune` - remove all unused containers
`docker run --network {network} {container}` - run {container}, being connected to {network}

**Other**
`docker ps --format "table {{.ID}}\t{{.Image}}\t{{.CreatedAt}}\t{{.Status}}\t{{.Names}}"` - customize docker ps output
`docker exec -it kafka bash` - run bash command in Kafka container
`docker exec -it kafka sh -c “cd opt; ls”` - run bash command in Kafka container
