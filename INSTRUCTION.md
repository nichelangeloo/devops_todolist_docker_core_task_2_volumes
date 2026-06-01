# Docker Setup Instructions

## Prerequisites

- Docker installed and running on your machine
- Docker Hub account (to pull images)

---

## 1. Run MySQL Container with Volume Attached

### Build MySQL image locally (optional, if not pulling from Docker Hub)

```bash
docker build -f Dockerfile.mysql -t mysql-local:1.0.0 .
```

### Run MySQL container with a named volume

```bash
docker run -d \
  --name mysql-container \
  -v mysql_data:/var/lib/mysql \
  -p 3306:3306 \
  mysql-local:1.0.0
```

**Explanation of flags:**

- `-d` — run in detached (background) mode
- `--name mysql-container` — give the container a recognizable name
- `-v mysql_data:/var/lib/mysql` — mount a named Docker volume to persist database data
- `-p 3306:3306` — expose MySQL port to the host

### Verify the container is running

```bash
docker ps
```

### Get the MySQL container IP address

```bash
docker network inspect bridge
```

Find our running container from the list and get IP address from there.

---

## 2. Run the App Container (connects to MySQL)

> **Important:** Make sure the MySQL container is running and healthy before starting the app container.

### Pull the app image from Docker Hub

```bash
docker pull <your-dockerhub-username>/todoapp:2.0.0
```

### Run the app container

```bash
docker run -d \
  --name todoapp-container \
  -p 8080:8080 \
  <your-dockerhub-username>/todoapp:2.0.0
```
---

## 3. Access the Application via Browser

Once both containers are running, open your browser and navigate to:

```
http://localhost:8080
```

If running on a remote server, replace `localhost` with the server's IP address:

```
http://<server-ip>:8080
```

---

## 4. Docker Hub Repository

The app images are available on Docker Hub:

https://hub.docker.com/repository/docker/nichelangeloo/todoapp/general

https://hub.docker.com/repository/docker/nichelangeloo/mysql-local/general

To pull the images directly:

```bash
docker pull <your-dockerhub-username>/todoapp:2.0.0

docker pull <your-dockerhub-username>/mysql-local:1.0.0
```
