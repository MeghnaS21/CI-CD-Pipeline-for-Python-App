# CI-CD-Pipeline-for-Python-App
This is the CI/CD pipeline for simple python app.

# 🐳 Docker Learning Notes

A complete reference guide for Docker — from a simple Python app to Flask + Redis with Docker Compose.

---

## 📁 Part 1: Simple Python App with Docker

### Project Structure

```
simple-python-app/
├── app.py
├── Dockerfile
```

### Step 1: Create the App

**app.py**
```python
print("Hello World!")
```

---

### Step 2: Write the Dockerfile

**Dockerfile**
```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY . .
CMD ["python", "app.py"]
```

#### Dockerfile Keywords Explained

| Keyword | What it does |
|---------|-------------|
| `FROM` | Base image to start from |
| `WORKDIR` | Sets working directory inside container |
| `COPY` | Copies files from your machine into container |
| `RUN` | Runs a command during image build |
| `CMD` | Command to run when container starts |
| `EXPOSE` | Documents which port the app uses |
| `ENV` | Sets environment variables |

---

### Step 3: Build the Docker Image

```bash
docker build -t app .
```

| Part | Meaning |
|------|---------|
| `docker build` | Build a Docker image |
| `-t app` | Name/tag the image as "app" |
| `.` | Use Dockerfile in current folder |

---

### Step 4: Run the Container

```bash
docker run -p 5000:5000 app
```

| Part | Meaning |
|------|---------|
| `docker run` | Start a container from an image |
| `-p 5000:5000` | Map port — outside:inside |
| `app` | Name of the image to run |

---

### Step 5: Useful Commands

```bash
# See all images
docker images

# Remove an image
docker rmi app

# Remove unused images
docker image prune

# See running containers
docker ps

# Stop a container
docker stop <container_id>
```

---

### Step 6: Multi-Stage Build (Reduce Image Size)

Use multi-stage builds to make your image smaller.

**Dockerfile (Multi-Stage)**
```dockerfile
# Stage 1 — Builder (heavy, temporary)
FROM python:3.10-slim AS builder
WORKDIR /app
COPY . .

# Stage 2 — Runner (light, final image)
FROM python:3.10-alpine
WORKDIR /app
COPY --from=builder /app .
CMD ["python", "app.py"]
```

#### Result

| Build Type | Size |
|-----------|------|
| Normal build | 122MB |
| Multi-stage build | 51.6MB ✅ |

---

## 📁 Part 2: Flask + Redis App with Docker Compose

### Project Structure

```
flask-compose-app/
├── app.py
├── requirements.txt
├── Dockerfile
└── docker-compose.yml
```

---

### Step 1: Create the Flask App

**app.py**
```python
from flask import Flask
import redis

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = cache.incr('hits')
    return f'Hello! Aapne {count} baar visit kiya!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

### Step 2: Add Dependencies

**requirements.txt**
```
flask
redis
```

---

### Step 3: Write the Dockerfile

**Dockerfile**
```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

---

### Step 4: Write the Docker Compose File

**docker-compose.yml**
```yaml
version: '3.8'

services:
  # Service 1 — Flask App
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  # Service 2 — Redis Database
  redis:
    image: redis:alpine
```

#### Docker Compose Keywords Explained

| Keyword | Meaning |
|---------|---------|
| `services` | List of containers to run |
| `build: .` | Build image from Dockerfile in current folder |
| `ports` | Map ports — outside:inside |
| `depends_on` | Start redis before web |
| `image` | Use a ready-made image from Docker Hub |

---

### Step 5: Run with Docker Compose

```bash
# Start all containers
docker-compose up

# Start in background (detached mode)
docker-compose up -d

# Stop all containers
docker-compose down

# See running containers
docker-compose ps

# See logs
docker-compose logs
```

---

### Step 6: Test the App

Open browser and go to:
```
http://localhost:5000
```

Every time you refresh, the visit count will increase — stored in Redis! ✅

---

## 🔄 Docker vs Docker Compose — When to Use What?

```
One container needed   →  docker run
Two+ containers needed →  docker-compose up
```

---

## 📌 Quick Command Reference

| Command | What it does |
|---------|-------------|
| `docker build -t name .` | Build image from Dockerfile |
| `docker run -p 5000:5000 name` | Run a container |
| `docker images` | List all images |
| `docker ps` | List running containers |
| `docker rmi name` | Remove an image |
| `docker image prune` | Remove unused images |
| `docker-compose up` | Start all services |
| `docker-compose down` | Stop all services |
| `docker-compose logs` | View logs |
