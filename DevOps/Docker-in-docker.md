**Docker-in-Docker (DinD)** is a setup where **a Docker container runs another Docker daemon inside it**, allowing you to create and manage containers from within a container.

**Use Case:**
- Running **Docker commands** inside CI/CD pipelines (e.g., **GitLab CI/CD, Jenkins**).
- **Building and pushing Docker images** inside a container.
- **Testing containerized applications** without affecting the host Docker daemon.

Normally, Docker **runs on the host machine** and manages containers.  
With **Docker-in-Docker**, a container **acts as a separate Docker host**.

`docker run --privileged -d --name docker-dind docker:dind`

This starts a container **running a full Docker daemon** inside.
Now you can run `docker build`, `docker run`, and `docker push` inside the container.

Instead of running a **nested** Docker daemon, you can use the **host's Docker daemon** by mounting `/var/run/docker.sock`

`docker run -v /var/run/docker.sock:/var/run/docker.sock docker`

**Pros of this approach:**
- **Less overhead** (no need for a separate Docker daemon).
- **Faster builds** (since it reuses the host’s Docker cache).

🔹 **Cons:**
- **Less isolation** (Docker commands affect the host system).