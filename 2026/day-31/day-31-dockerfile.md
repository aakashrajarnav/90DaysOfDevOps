# Day 31 – Dockerfile: Build Your Own Images

## Challenge Tasks

### Task 1: Your First Dockerfile
1. Create a folder called `my-first-image`
```bash
mkdir my-first-image
cd my-first-image
```
2. Inside it, create a `Dockerfile` that:
   - Uses `ubuntu` as the base image
   - Installs `curl`
   - Sets a default command to print `"Hello from my custom image!"`
```text
vim Dockerfile
```

```bash
from ubuntu:latest

RUN apt-get update && apt-get install curl -y

CMD ["echo", "Hello from my custom image!"]
```
3. Build the image and tag it `my-ubuntu:v1`
```bash
docker build -t my-ubuntu:v1 .
```
4. Run a container from your image
```bash
docker run my-ubuntu:v1
```
**Verify:** The message prints on `docker run`

---

### Task 2: Dockerfile Instructions
Create a new Dockerfile that uses **all** of these instructions:
- `FROM` — base image
- `RUN` — execute commands during build
- `COPY` — copy files from host to image
- `WORKDIR` — set working directory
- `EXPOSE` — document the port
- `CMD` — default command

Build and run it. Understand what each line does.

```bash
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

COPY . .

EXPOSE 80

# Compile directly inside src
RUN javac src/todolist/ToDoList.java

# Run with src as classpath
CMD ["java", "-cp", "src", "todolist.ToDoList"]
```

---

### Task 3: CMD vs ENTRYPOINT
1. Create an image with `CMD ["echo", "hello"]` — run it, then run it with a custom command. What happens?

```bash
mkdir my-third-image
cd my-third-image
vim Dockerfile
```

```Dockerfile
FROM ubuntu:latest
CMD ["echo", "hello"]
```

```bash
docker build -t cmd-image:v1 .
docker run cmd-image:v1
```

```output
output: hello
```

```bash
docker run cmd-image:v1 echo "Hi Aakash"
```

```output
output: Hi Aakash
```

2. Create an image with `ENTRYPOINT ["echo"]` — run it, then run it with additional arguments. What happens?

```bash
mkdir my-fourth-image
cd my-fourth-image
vim Dockerfile
```

```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo", "hello"]
```

```bash
docker build -t entrypoint-image:v1 .
docker run entrypoint-image:v1
```

```output
output: hello
```

```bash
docker run entrypoint-image:v1 echo "Hi Aakash"
```

```output
output: hello Hi Aakash
```

3. Write in your notes: When would you use CMD vs ENTRYPOINT?

**When to Use CMD**

- When you want a default command

- When users may override it

- For simple containers

**When to Use ENTRYPOINT**

- When container should behave like an executable

- When you want to enforce a specific command

- For CLI tools

---

### Task 4: Build a Simple Web App Image
1. Create a small static HTML file (`index.html`) with any content

```bash
mkdir indeximage
cd indeximage
vim index.html
```

```bash
<!DOCTYPE html>
<html>
<head>
    <title>My Docker Image</title>
</head>
<body>
    <h1>Hello Aakash</h1>
    <p>I am a Software Designer</p>
</body>
</html>
```

2. Write a Dockerfile that:
   - Uses `nginx:alpine` as base
   - Copies your `index.html` to the Nginx web directory

```bash
vim Dockerfile
```

```bash
# Pull nginx alpine image
FROM nginx:alpine

# Copy static page to Nginx default directory
COPY index.html /usr/share/nginx/html/

# Expose port 80
EXPOSE 80
```

3. Build and tag it `my-website:v1`

```bash
docker build -t my-website:v1 .
```
4. Run it with port mapping and access it in your browser

```bash
docker run -d -p 8080:80 --name nginx my-website:v1
```
---

### Task 5: .dockerignore
1. Create a `.dockerignore` file in one of your project folders
```bash
vim .dockerignore
```
2. Add entries for: `node_modules`, `.git`, `*.md`, `.env`
```bash
node_modules
.git
*.md
.env
```
3. Build the image — verify that ignored files are not included

Create dummy files/folder and build it
```bash
mkdir node_modules
touch test.md
touch .env
docker build -t my-website:v2 .
```

Look at the first line:

**Sending build context to Docker daemon  X.XMB**

---

### Task 6: Build Optimization
1. Build an image, then change one line and rebuild — notice how Docker uses **cache**

vim Dockerfile
```bash
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install curl -y
COPY index.html /app/
```

```bash
docker build -t cache-image:v1 .
```

Now change one line in index.html

```bash
docker build -t cache-test:v2 .
```

```text
Using cache
Using cache
Using cache
Step 4/4 : COPY index.html
```

```text

How Docker Cache Works

Docker builds layer by layer.

Each instruction = one layer.

If a layer doesn't change => Docker reuses cache.

If one layer changes => all layers AFTER it rebuild.

```

2. Reorder your Dockerfile so that frequently changing lines come **last**

```bash
COPY index.html /app/
RUN apt-get install curl -y
```

```text
If change in file => COPY changes => curl install runs again (slow)
```

---

```bash
RUN apt-get install curl -y
COPY index.html /app/
```

```text
- If source code changes → only last COPY rebuilds

- Dependencies stay cached

- Much faster build
```

3. Write in your notes: Why does layer order matter for build speed?

- Docker caches layers sequentially

- If one layer changes => all subsequent layers rebuild

- Frequently changing files should be placed last

- Stable operations (like installing dependencies) should be earlier

```text
In .NET apps:

- Copy .csproj

- Run dotnet restore

- Copy rest of code

- Run dotnet build

This reduces rebuild time from minutes → seconds.
```