**Docker-in-Docker (DinD)** is a setup where **a Docker container runs another Docker daemon inside it**, allowing you to create and manage containers from within a container.

**Use Case:**
- Running **Docker commands** inside CI/CD pipelines (e.g., **GitLab CI/CD, Jenkins**).
- **Building and pushing Docker images** inside a container.
- **Testing containerized applications** without affecting the host Docker daemon.

Normally, Docker runs a daemon that manages containers on the host machine. With **Docker-in-Docker**, a container acts as a separate Docker host with its own daemon withinTo work this container need special permissions to the real host kernel These permissions are granted by the flag `--privileged` that disables all security mechanisms like cgroups, AppArmor, SECCOMP.

`docker run --privileged -d --name docker-dind docker:dind`

This starts a container with a full Docker daemon running inside. This container can run `docker build`, `docker run`, and `docker push` inside the container.

Now this approach is considered obsolete because of security issues. The industry nowadays uses [[Docker-out-of-Docker]] instead.

