# Day 32 – Docker Volumes & Networking

## Task
Today's goal is to **solve two real problems: data persistence and container communication**.

### Task 1: The Problem
1. Run a Postgres or MySQL container

```bash
docker run -d -p 5432 --name postgres -e POSTGRES_PASSWORD=admin postgres:latest
```

2. Create some data inside it (a table, a few rows — anything)

```bash
docker exec -it postgres psql -U postgres

\l #list database

CREATE DATABASE EMPLOYEE;

\c employee #switch to database

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT);

\dt # list lables

INSERT INTO users (name) VALUES ('Aakash'), ('Raj');

SELECT * FROM users;

\q #quit
```
3. Stop and remove the container

```bash
docker stop postgres && docker rm postgres
```
4. Run a new one — is your data still there?
```bash
docker run -d -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=admin postgres:latest

docker exec -it postgres psql -U postgres

\l
```

No data is not there, When you remove the container, its internal filesystem is deleted.

Write what happened and why?
Docker containers are short-lived. When a container is removed, its writable layer is deleted

---

### Task 2: Named Volumes
1. Create a named volume

```bash
docker volume create vol

docker volume list
```
2. Run the same database container, but this time **attach the volume** to it
```bash
docker run -d -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=admin -v vol:/var/lib/postgresql postgres:latest
```
/var/lib/postgresql/data is where PostgreSQL stores all database files.

3. Add some data, stop and remove the container
```bash
docker exec -it postgres psql -U postgres

\l #list database

CREATE DATABASE EMPLOYEE;

\c employee #switch to database

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT);

\dt # list tables

INSERT INTO users (name) VALUES ('Aakash'), ('Raj');

SELECT * FROM users;

\q #quit

docker stop postgres && docker rm postgres
```
4. Run a brand new container with the **same volume**
```bash
docker run -d -p 5432:5432 --name postgres_new -e POSTGRES_PASSWORD=admin -v vol:/var/lib/postgresql postgres:latest
```
5. Is the data still there?
```bash
docker exec -it postgres_new psql -U postgres

\l

\c employee

SELECT * FROM users;
```
**Verify:** `docker volume ls`, `docker volume inspect`

```text
docker volume ls
vol
docker volume inspect vol
[
    {
        "CreatedAt": "2026-02-28T13:01:25Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/vol/_data",
        "Name": "vol",
        "Options": null,
        "Scope": "local"
    }
]

When you use:

-v vol:/var/lib/postgresql

Docker:

- Creates (or reuses) a volume named vol(my volume name)

- Mounts it into the container

- PostgreSQL writes database files there

- Removing the container does NOT delete the volume
```

---

### Task 3: Bind Mounts
1. Create a folder on your host machine with an `index.html` file
```bash
mkdir mywebsite
cd mywebsite

vim index.html
```

```text
<!DOCTYPE html>
<html>
<head>
  <title>Bind Mount Test</title>
</head>
<body>
  <h1>Hello from Aakash</h1>
</body>
</html>
```
2. Run an Nginx container and **bind mount** your folder to the Nginx web directory
```bash
docker run -d --name nginx -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx:latest
```
Nginx serve file from /usr/share/nginx/html
3. Access the page in your browser
```bash
http://localhost:8080
```
4. Edit the `index.html` on your host — refresh the browser
```bash
vim index.html
<h1>Hello from Aakash Raj(updated)</h1>
```

Write in your notes: What is the difference between a named volume and a bind mount?
- A named volume is managed by Docker and abstracts the storage location, making it portable and production-safe. UseCase: Database persistence (Postgres, MySQL)
- A bind mount directly maps a host directory into a container, making it ideal for development but tightly coupled to the host filesystem. Usecase: Frontend development
---

### Task 4: Docker Networking Basics
1. List all Docker networks on your machine
```bash
docker network list
```
2. Inspect the default `bridge` network
```bash
docker network inspect bridge
```
3. Run two containers on the default bridge — can they ping each other by **name**?
```bash
docker run -d -it --name test1 alpine sh
docker run -d -it --name test2 alpine sh
c95ad7b079b5fa799385d2700362c9ab1f377c28fd7c88cf2dadf33db9459dde
docker exec -it test1 sh
apk add iputils
docker exec -it test2 sh
apk add iputils
ping test1
Output: ping: test1: Try again
```

4. Run two containers on the default bridge — can they ping each other by **IP**?

```bash
docker inspect test1 | grep IPAddress
docker inspect test2 | grep IPAddress
docker exec -it test1 sh
ping 172.17.0.5 # test2 container ip

docker exec -it test2 sh
ping 172.17.0.4 # test1 container ip
```

Default bridge allows container-to-container communication but only via IP

---

### Task 5: Custom Networks
1. Create a custom bridge network called `my-app-net`
```bash
docker network create my-app-net
```
2. Run two containers on `my-app-net`
```
docker run -d -it --name app1 --network my-app-net alpine sh
docker run -d -it --name app2 --network my-app-net alpine sh
```
3. Can they ping each other by **name** now?
Yes
```bash
docker run -d -it --name app1 --network my-app-net alpine sh
docker run -d -it --name app2 --network my-app-net alpine sh

docker exec -it app1 sh
ping app2

# Same with App1 inside App2
docker exec -it app2 sh
ping app1
```

4. Write in your notes: Why does custom networking allow name-based communication but the default bridge doesn't?
Because user-defined bridge networks include Docker’s embedded DNS server.

---

### Task 6: Put It Together
1. Create a custom network
```bash
docker network create my-app-net
```
2. Run a **database container** (MySQL/Postgres) on that network with a volume for data
```bash
docker run -d --name postgres --network my-app-net -e POSTGRES_PASSWORD=admin -v vol:/var/lib/postgresql postgres:latest
```
3. Run an **app container** (use any image) on the same network
```bash
docker run -d -it --name app1 --network my-app-net alpine sh
```
4. Verify the app container can reach the database by container name
```bash
docker exec -it app1 sh
apk add postgresql-client # installing postgresql client
psql -h postgres -U postgres
password: ******
\q

ping postgres # container name of postgres
```