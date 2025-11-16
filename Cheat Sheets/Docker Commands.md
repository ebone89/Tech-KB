# Docker Commands Cheat Sheet

Essential Docker commands for container management.

## 🐳 Docker Basics

```bash
# Check version
docker --version
docker version
docker info

# Get help
docker --help
docker run --help
```

## 📦 Image Management

```bash
# List images
docker images
docker image ls

# Pull image from registry
docker pull ubuntu
docker pull nginx:latest
docker pull mysql:8.0

# Build image from Dockerfile
docker build -t myapp:1.0 .
docker build -t myapp:latest -f Dockerfile.prod .
docker build --no-cache -t myapp:1.0 .

# Tag image
docker tag myapp:1.0 myapp:latest
docker tag myapp:1.0 myregistry.com/myapp:1.0

# Push image to registry
docker push myregistry.com/myapp:1.0

# Remove image
docker rmi image_name
docker rmi image_id
docker rmi -f image_id                # Force remove

# Remove unused images
docker image prune
docker image prune -a                 # Remove all unused images

# Inspect image
docker image inspect nginx
docker history nginx                  # Show layers
```

## 🚀 Container Management

### Running Containers
```bash
# Run container
docker run nginx
docker run -d nginx                   # Detached mode
docker run -d --name mycontainer nginx
docker run -it ubuntu bash            # Interactive terminal

# Port mapping
docker run -d -p 8080:80 nginx        # Host:Container
docker run -d -p 127.0.0.1:8080:80 nginx

# Environment variables
docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql
docker run -d --env-file .env myapp

# Volume mounting
docker run -d -v /host/path:/container/path nginx
docker run -d -v myvolume:/data nginx
docker run -d --mount type=bind,source=/host,target=/container nginx

# Network
docker run -d --network mynetwork nginx
docker run -d --network host nginx

# Resource limits
docker run -d --memory="512m" --cpus="1.0" nginx

# Auto-restart
docker run -d --restart unless-stopped nginx
docker run -d --restart always nginx
```

### Managing Running Containers
```bash
# List containers
docker ps                             # Running only
docker ps -a                          # All containers
docker ps -q                          # IDs only
docker ps --filter "status=exited"

# Start/Stop/Restart
docker start container_name
docker stop container_name
docker restart container_name
docker pause container_name
docker unpause container_name

# Remove container
docker rm container_name
docker rm -f container_name           # Force remove running
docker rm $(docker ps -aq)            # Remove all stopped

# Container info
docker inspect container_name
docker logs container_name
docker logs -f container_name         # Follow logs
docker logs --tail 100 container_name
docker stats                          # Resource usage
docker top container_name             # Running processes
```

### Interacting with Containers
```bash
# Execute command in container
docker exec container_name ls /app
docker exec -it container_name bash
docker exec -it container_name sh

# Attach to container
docker attach container_name

# Copy files
docker cp container_name:/path/file.txt ./local/
docker cp ./local/file.txt container_name:/path/

# View container changes
docker diff container_name

# Export/Import container
docker export container_name > backup.tar
docker import backup.tar myimage:latest
```

## 🗄️ Volume Management

```bash
# List volumes
docker volume ls

# Create volume
docker volume create myvolume

# Inspect volume
docker volume inspect myvolume

# Remove volume
docker volume rm myvolume
docker volume prune                   # Remove unused volumes

# Use volume
docker run -d -v myvolume:/data nginx
```

## 🌐 Network Management

```bash
# List networks
docker network ls

# Create network
docker network create mynetwork
docker network create --driver bridge mynetwork
docker network create --subnet=172.18.0.0/16 mynetwork

# Inspect network
docker network inspect mynetwork

# Connect/Disconnect container
docker network connect mynetwork container_name
docker network disconnect mynetwork container_name

# Remove network
docker network rm mynetwork
docker network prune                  # Remove unused networks
```

## 📝 Dockerfile

### Basic Dockerfile
```dockerfile
# Base image
FROM ubuntu:20.04

# Metadata
LABEL maintainer="your@email.com"
LABEL version="1.0"

# Set working directory
WORKDIR /app

# Copy files
COPY . /app
COPY requirements.txt .

# Run commands
RUN apt-get update && \
    apt-get install -y python3 && \
    apt-get clean

# Install dependencies
RUN pip install -r requirements.txt

# Expose port
EXPOSE 8080

# Environment variables
ENV APP_ENV=production
ENV PORT=8080

# Default command
CMD ["python3", "app.py"]
# or
ENTRYPOINT ["python3", "app.py"]
```

### Multi-stage Build
```dockerfile
# Build stage
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

## 🐙 Docker Compose

### Basic docker-compose.yml
```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - mynetwork
    restart: unless-stopped

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: mydb
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - mynetwork

volumes:
  db_data:

networks:
  mynetwork:
```

### Docker Compose Commands
```bash
# Start services
docker-compose up
docker-compose up -d                  # Detached
docker-compose up --build             # Rebuild images

# Stop services
docker-compose down
docker-compose down -v                # Remove volumes
docker-compose stop

# View services
docker-compose ps
docker-compose logs
docker-compose logs -f service_name

# Execute command
docker-compose exec web bash
docker-compose run web python manage.py migrate

# Scale services
docker-compose up -d --scale web=3

# Build images
docker-compose build
docker-compose build --no-cache
```

## 🔍 Troubleshooting

```bash
# View logs
docker logs container_name
docker logs -f --tail 100 container_name

# Inspect container/image
docker inspect container_or_image
docker inspect --format='{{.State.Status}}' container

# Check resource usage
docker stats
docker stats --no-stream

# System information
docker system df                      # Disk usage
docker system info
docker system events                  # Real-time events

# Clean up
docker system prune                   # Remove unused data
docker system prune -a                # Remove all unused
docker system prune -a --volumes      # Include volumes

# Debug container
docker exec -it container_name sh
docker run -it --entrypoint sh image_name
```

## 💡 Useful Commands

```bash
# Remove all stopped containers
docker container prune
docker rm $(docker ps -aq -f status=exited)

# Remove all dangling images
docker image prune
docker rmi $(docker images -f "dangling=true" -q)

# Stop all running containers
docker stop $(docker ps -q)

# Remove all containers
docker rm -f $(docker ps -aq)

# Show container IP address
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name

# Monitor logs from multiple containers
docker-compose logs -f service1 service2

# Create container from image and run command
docker run --rm -it ubuntu bash

# Copy file from stopped container
docker cp container_name:/path/file.txt ./

# Export and compress image
docker save myimage:latest | gzip > myimage.tar.gz

# Load compressed image
gunzip -c myimage.tar.gz | docker load

# Show port mappings
docker port container_name

# Wait for container to exit
docker wait container_name
```

## 🎯 Best Practices

1. **Use official images**: Start with official Docker Hub images
2. **Use specific tags**: Don't use `latest` in production
3. **Minimize layers**: Combine RUN commands with `&&`
4. **Use .dockerignore**: Exclude unnecessary files
5. **Multi-stage builds**: Reduce final image size
6. **Don't run as root**: Use USER directive
7. **Health checks**: Add HEALTHCHECK to Dockerfile
8. **Label your images**: Use LABEL for metadata
9. **Clean up**: Remove containers and images regularly
10. **Security scanning**: Use `docker scan image_name`

## 📋 Quick Reference

| Command | Description |
|---------|-------------|
| `docker run` | Create and start container |
| `docker ps` | List running containers |
| `docker stop` | Stop container |
| `docker rm` | Remove container |
| `docker images` | List images |
| `docker rmi` | Remove image |
| `docker pull` | Download image |
| `docker build` | Build image from Dockerfile |
| `docker exec` | Execute command in container |
| `docker logs` | View container logs |
| `docker-compose up` | Start services |
| `docker-compose down` | Stop services |

## Related Topics

- [[Development/Development Index|Development]]
- [[Linux/Common Commands|Linux Commands]]
- [[Tips and Tricks/Command Line Productivity|Command Line Productivity]]

---

#docker #containers #devops #cheatsheet #reference
