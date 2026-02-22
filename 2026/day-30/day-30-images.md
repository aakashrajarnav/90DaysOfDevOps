# Day 30 – Docker Images & Container Lifecycle

## Challenge Tasks

### Task 1: Docker Images
1. Pull the `nginx`, `ubuntu`, and `alpine` images from Docker Hub

```bash
docker pull nginx:latest
docker pull ubuntu:latest
docker pull alpine:latest
```

2. List all images on your machine — note the sizes
```text
ubuntu                        latest         bbdabce66f1b   12 days ago         78.1MB
nginx                         latest         5cdef4ac3335   2 weeks ago         161MB
alpine                        latest         a40c03cbb81c   3 weeks ago         8.44MB
```
3. Compare `ubuntu` vs `alpine` — why is one much smaller?

| Feature            | Ubuntu           | Alpine                 |
|--------------------|------------------|------------------------|
| C library          | glibc            | musl                   |
| Shell              | bash             | ash                    |
| Package manager    | apt              | apk                    |
| Base philosophy    | Full Linux OS    | Minimal container OS   |
| Size               | Large            | Very Small             |

4. Inspect an image — what information can you see?
```bash
docker image inspect ubuntu
```

```text
Image ID and Repo Tags and Repo digest

Creation time

Docker version

Author

OS and CPU architecture

Environment variables

Default CMD

Working directory

Image size

Labels and metadata
```
5. Remove an image you no longer need

```bash
docker rmi alpine
```
---

### Task 2: Image Layers
1. Run `docker image history nginx` — what do you see?
```text
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
5cdef4ac3335   2 weeks ago   CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   COPY 30-tune-worker-processes.sh /docker-ent…   4.62kB    buildkit.dockerfile.v0
<missing>      2 weeks ago   COPY 20-envsubst-on-templates.sh /docker-ent…   3.02kB    buildkit.dockerfile.v0
<missing>      2 weeks ago   COPY 15-local-resolvers.envsh /docker-entryp…   389B      buildkit.dockerfile.v0
<missing>      2 weeks ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   2.12kB    buildkit.dockerfile.v0
<missing>      2 weeks ago   COPY docker-entrypoint.sh / # buildkit          1.62kB    buildkit.dockerfile.v0
<missing>      2 weeks ago   RUN /bin/sh -c set -x     && groupadd --syst…   82.2MB    buildkit.dockerfile.v0
<missing>      2 weeks ago   ENV DYNPKG_RELEASE=1~trixie                     0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   ENV PKG_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   ENV ACME_VERSION=0.3.1                          0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   ENV NJS_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   ENV NJS_VERSION=0.9.5                           0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   ENV NGINX_VERSION=1.29.5                        0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      2 weeks ago   # debian.sh --arch 'amd64' out/ 'trixie' '@1…   78.6MB    debuerreotype 0.17
```
- Each row represents **one image layer**
- Layers are created from Dockerfile instructions like:
  - `FROM`
  - `RUN`
  - `COPY`
  - `ENV`
  - `CMD`
  - `ENTRYPOINT`
- Some layers have a **size (MB/kB)** → they changed the filesystem
- Some layers show **0B** → metadata only (no filesystem change)
- Bottom layer = base OS (Debian)
- Top layer = final command (`CMD`)

2. Each line is a **layer**. Note how some layers show sizes and some show 0B
- `RUN`, `COPY`, `ADD` → create filesystem changes → have size
- `ENV`, `LABEL`, `CMD`, `ENTRYPOINT`, `EXPOSE` → metadata only → 0B

Example:
- `RUN apt install` → adds files → large size
- `ENV VERSION=1.0` → metadata only → 0B

3. Write in your notes: What are layers and why does Docker use them?
**Docker layers** are immutable filesystem snapshots created for each Dockerfile instruction. They enable caching, reuse, faster builds, and efficient image distribution.

---

### Task 3: Container Lifecycle
Practice the full lifecycle on one container:
1. **Create** a container (without starting it)
2. **Start** the container
3. **Pause** it and check status
4. **Unpause** it
5. **Stop** it
6. **Restart** it
7. **Kill** it
8. **Remove** it

Check `docker ps -a` after each step — observe the state changes.

#### 1. Create a container (without starting it)

```bash
docker create --name mycontainer nginx
```

Check status:

```bash
docker ps -a
```

State → `Created`

---

#### 2. Start the container

```bash
docker start mycontainer
```

Check:

```bash
docker ps -a
```

State → `Up` (Running)

---

#### 3. Pause the container

```bash
docker pause mycontainer
```

Check:

```bash
docker ps -a
```

State → `Paused`

---

#### 4. Unpause the container

```bash
docker unpause mycontainer
```

Check:

```bash
docker ps -a
```

State → `Up` (Running)

---

#### 5. Stop the container

```bash
docker stop mycontainer
```

Check:

```bash
docker ps -a
```

State → `Exited`

---

#### 6. Restart the container

```bash
docker restart mycontainer
```

Check:

```bash
docker ps -a
```

State → `Up` (Running)

---

#### 7. Kill the container

```bash
docker kill mycontainer
```

Check:

```bash
docker ps -a
```

State → `Exited`

---

#### 8. Remove the container

```bash
docker rm mycontainer
```

Check:

```bash
docker ps -a
```

Container should no longer appear.

#### Container states summary

- `Created` → Container exists but not running
- `Up` → Running
- `Paused` → Processes frozen
- `Exited` → Stopped
- `Removed` → No longer exists

---

### Task 4: Working with Running Containers
1. Run an Nginx container in detached mode
```bash
docker run -d -p 80:80 --name mynginx nginx:latest
```
2. View its **logs**
```bash
docker logs mynginx
```
3. View **real-time logs** (follow mode)
```bash
docker logs -f mynginx
```
4. **Exec** into the container and look around the filesystem
```bash
docker exec -it mynginx bash
ls
cd /home
exit
```
5. Run a single command inside the container without entering it
```bash
docker exec mynginx ls /usr/share/nginx/html
```
6. **Inspect** the container — find its IP address, port mappings, and mounts
```bash
docker container inspect mynginx
```
We can find IP addesses, port mappings and mounts there, There is a Shortcuts also :

- **IP Address**

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mynginx
```

- **Port Mappings**

```bash
docker port mynginx
```

- **Mounts**

```bash
docker inspect -f '{{.Mounts}}' mynginx
```

---

### Task 5: Cleanup
1. Stop all running containers in one command
```bash
docker stop $(docker ps -q)
```
- `docker ps -q` → returns only container IDs
- `docker stop` → stops them

2. Remove all stopped containers in one command
```bash
docker container prune
```
3. Remove unused images
```bash
docker image prune
```
4. Check how much disk space Docker is using
```bash
docker system df
```
OR **for detailed view**
```bash
docker system df -v
```

- **Remove everything unused (containers, images, networks, cache):**

```bash
docker system prune -a
```
