# Day 33 – Docker Compose: Multi-Container Basics

## Task
Today's goal is to **run multi-container applications with a single command**.

---

## Challenge Tasks

### Task 1: Install & Verify
1. Check if Docker Compose is available on your machine
2. Verify the version
```bash
docker compose version
```
if not available
```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin
```
---

### Task 2: Your First Compose File
1. Create a folder `compose-basics`
```bash
mkdir compose-basics
cd compose-basics
```
2. Write a `docker-compose.yml` that runs a single **Nginx** container with port mapping
```bash
vim docker-compose.yml

version: "3.9"

services:
  nginx:
    image: nginx:latest
    container_name: nginx-server
    ports:
      - "8080:80"
```
3. Start it with `docker compose up`
```bash
docker compose up
```
4. Access it in your browser
```bash
http://localhost:8080
```
5. Stop it with `docker compose down`
```bash
docker compose down
```
---

### Task 3: Two-Container Setup
Write a `docker-compose.yml` that runs:
- A **WordPress** container
- A **MySQL** container

They should:
- Be on the same network (Compose does this automatically)
- MySQL should have a named volume for data persistence
- WordPress should connect to MySQL using the service name

Start it, access WordPress in your browser, and set it up.

**Verify:** Stop and restart with `docker compose down` and `docker compose up` — is your WordPress data still there?

```bash
version: "3.9"

services:
  db:
    image: mysql:8
    container_name: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: admin
      MYSQL_DATABASE: wordpress
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
    volumes:
      - mysql_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress
    restart: always
    ports:
      - "8081:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: admin
      WORDPRESS_DB_PASSWORD: admin
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  mysql_data:
```

Yes, data is still there because of named volume

---

### Task 4: Compose Commands
Practice and document these:
1. Start services in **detached mode**
```bash
docker compose up -d
```
2. View running services
```bash
docker compose ps
```
3. View **logs** of all services
```bash
docker compose logs
docker compose logs -f # live logs
```
4. View logs of a **specific** service
```bash
docker compose logs wordpress
or
docker logs wordpress


docker compose logs db
or
docker logs wordpress
```
5. **Stop** services without removing
```bash
docker compose stop
```
6. **Remove** everything (containers, networks)
```bash
docker compose down
```
7. **Rebuild** images if you make a change
```bash
docker compose up --build
or
docker compose up -d --build
```
---

### Task 5: Environment Variables
1. Add environment variables directly in your `docker-compose.yml`
```bash
vim docker-compose.yml

version: "3.9"

services:
  app:
    image: alpine
    container_name: env
    command: sh -c "env && sleep 10"
    environment:
      APP_ENV: development
      APP_DEBUG: "true"


docker compose up
```
2. Create a `.env` file and reference variables from it in your compose file
```bash
vim .env

APP_ENV=production
APP_DEBUG=false

```

```bash
services:
  app:
    services:
  env:
    image: alpine
    container_name: env
    command: sh -c "env && sleep 3600"
    environment:
      APP_ENV: ${APP_ENV}
      APP_DEBUG: ${APP_DEBUG}

docker compose up
```
3. Verify the variables are being picked up
```bash
docker exec -it env sh
printenv | grep APP
```
```text
APP_DEBUG=false
APP_ENV=production
```

