# Docker Commands Cheat Sheet

## 🐳 Docker Info & Status
| Command | Description |
|---------|-------------|
| `docker --version` | Check installed Docker version |
| `docker info` | Show system-wide Docker details |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped) |
| `docker images` | List all downloaded images |
| `docker stats` | Live resource usage of containers |

---

## 📦 Images
| Command | Description |
|---------|-------------|
| `docker pull <image>` | Download an image from Docker Hub |
| `docker build -t <name> .` | Build image from Dockerfile |
| `docker rmi <image>` | Remove an image |
| `docker tag <src> <target>` | Rename / retag an image |

---

## ▶️ Containers (Run / Stop)
| Command | Description |
|---------|-------------|
| `docker run <image>` | Run a container from an image |
| `docker run -it <image> bash` | Run in interactive shell |
| `docker run -d <image>` | Run container in background (detached) |
| `docker stop <container>` | Stop a running container |
| `docker start <container>` | Start a stopped container |
| `docker restart <container>` | Restart a container |
| `docker rm <container>` | Remove a container |

---

## 🛠 Container Operations
| Command | Description |
|---------|-------------|
| `docker logs <container>` | View container logs |
| `docker exec -it <container> bash` | Enter running container shell |
| `docker inspect <container>` | Detailed container info (JSON) |
| `docker cp file <container>:/path` | Copy file into container |
| `docker cp <container>:/path file` | Copy file out of container |

---

## 📂 Volumes & Networks
| Command | Description |
|---------|-------------|
| `docker volume ls` | List all volumes |
| `docker volume rm <name>` | Remove a volume |
| `docker network ls` | List networks |
| `docker network create <name>` | Create a network |

---

## 🧹 Cleanup
| Command | Description |
|---------|-------------|
| `docker system prune` | Remove unused data (images, containers, etc.) |
| `docker image prune` | Remove unused images |
| `docker container prune` | Remove stopped containers |

---

## 🐳 Docker Compose (optional)
| Command | Description |
|---------|-------------|
| `docker compose up -d` | Start all services (detached) |
| `docker compose down` | Stop & remove services |
| `docker compose logs` | View logs from all services |
| `docker compose ps` | Show running compose containers |

---
