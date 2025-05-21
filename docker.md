# Docker Setup Guide for Barter Trading System

This guide provides detailed instructions for setting up and running the Barter Trading System using Docker.

## Basic Docker Commands

### 1. Network Management

Create a new Docker network:
```bash
docker network create my-network
```

List all networks:
```bash
docker network ls
```

Inspect network details:
```bash
docker network inspect my-network
```

Remove a network:
```bash
docker network rm my-network
```

### 2. MongoDB Container

Start MongoDB container:
```bash
docker run --network my-network -p 27017:27017 --name mongodb mongo
```

Check MongoDB container status:
```bash
docker ps | grep mongodb
```

View MongoDB logs:
```bash
docker logs mongodb
```

Stop MongoDB container:
```bash
docker stop mongodb
```

Remove MongoDB container:
```bash
docker rm mongodb
```

### 3. Application Container

Build the application image:
```bash
docker build -t thenusan/barter-trading-system .
```

Run the application container:
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://mongodb:27017/barter-trading \
  thenusan/barter-trading-system
```

Check application container status:
```bash
docker ps | grep barter-trading-system
```

View application logs:
```bash
docker logs <container_id>
```

Stop application container:
```bash
docker stop <container_id>
```

Remove application container:
```bash
docker rm <container_id>
```

### 4. Troubleshooting Commands

Check all running containers:
```bash
docker ps
```

Check all containers (including stopped):
```bash
docker ps -a
```

Check container logs:
```bash
docker logs <container_id>
```

Check container details:
```bash
docker inspect <container_id>
```

Test MongoDB connection:
```bash
docker exec -it mongodb mongosh --eval "db.runCommand({ ping: 1 })"
```

### 5. Cleanup Commands

Stop all running containers:
```bash
docker stop $(docker ps -q)
```

Remove all stopped containers:
```bash
docker rm $(docker ps -a -q)
```

Remove all unused images:
```bash
docker image prune -a
```

Remove all unused networks:
```bash
docker network prune
```

## Common Issues and Solutions

### Port Conflict

If port 3000 is already in use:
```bash
# Find container using port 3000
docker ps | grep 3000

# Stop the container
docker stop <container_id>
```

### MongoDB Connection Issues

If you see "Error connecting to MongoDB: getaddrinfo ENOTFOUND mongodb":

1. Verify MongoDB is running:
```bash
docker ps | grep mongodb
```

2. Check network connectivity:
```bash
docker network inspect my-network
```

3. Test MongoDB connection:
```bash
docker exec -it mongodb mongosh --eval "db.runCommand({ ping: 1 })"
```

4. Restart with correct container name:
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://<actual_container_name>:27017/barter-trading \
  thenusan/barter-trading-system
```

### Container Not Starting

If the container fails to start:
```bash
# Check container logs
docker logs <container_id>

# Check container status
docker ps -a | grep <container_id>

# Remove and recreate container
docker rm <container_id>
docker run [previous run command]
```

## Best Practices

1. Always use named containers for easier management:
```bash
docker run --name mongodb ...
docker run --name barter-app ...
```

2. Use environment variables for configuration:
```bash
docker run -e MONGODB_URI=... -e PORT=... ...
```

3. Keep containers on the same network:
```bash
docker run --network my-network ...
```

4. Use volume mounts for persistent data:
```bash
docker run -v /host/path:/container/path ...
```

5. Clean up unused resources regularly:
```bash
docker system prune
```

## Security Considerations

1. Don't expose MongoDB port to host machine in production:
```bash
# Remove -p 27017:27017 in production
docker run --network my-network --name mongodb mongo
```

2. Use Docker secrets for sensitive data:
```bash
docker secret create mongodb_password ./password.txt
```

3. Run containers with non-root user:
```bash
docker run --user 1000:1000 ...
```

## Monitoring

Check container resource usage:
```bash
docker stats
```

Monitor container logs in real-time:
```bash
docker logs -f <container_id>
```

Check container health:
```bash
docker inspect --format='{{.State.Health.Status}}' <container_id>
```

# Step-by-Step Guide: Dockerizing Barter Trading System

This guide will walk you through the process of dockerizing and running the Barter Trading System with MongoDB.

## Step 1: Clone and Prepare the Project

1. Clone the repository:
```bash
git clone <repository-url>
cd TradeSystem
```

2. Ensure you have Docker installed:
```bash
docker --version
```

## Step 2: Create Docker Network

1. Create a network for container communication:
```bash
docker network create my-network
```

2. Verify network creation:
```bash
docker network ls
```

## Step 3: Set Up MongoDB Container

1. Start MongoDB container:
```bash
docker run --network my-network \
  -p 27017:27017 \
  --name mongodb \
  mongo
```

2. Verify MongoDB is running:
```bash
docker ps | grep mongodb
```

3. Test MongoDB connection:
```bash
docker exec -it mongodb mongosh --eval "db.runCommand({ ping: 1 })"
```

## Step 4: Build Application Docker Image

1. Create a Dockerfile in your project root (if not exists):
```bash
touch Dockerfile
```

2. Add the following content to Dockerfile:
```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]
```

3. Build the Docker image:
```bash
docker build -t thenusan/barter-trading-system .
```

4. Verify image creation:
```bash
docker images | grep barter-trading-system
```

## Step 5: Run the Application Container

1. Run the application container (Option 1 - with container name):
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://mongodb:27017/barter-trading \
  --name barter-app \
  thenusan/barter-trading-system
```

OR

Run the application container (Option 2 - without container name):
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://mongodb:27017/barter-trading \
  thenusan/barter-trading-system
```

2. Verify the application is running:
```bash
docker ps | grep barter-trading-system
```

3. Check application logs:
```bash
docker logs <container_id>
```

## Step 6: Verify Everything is Working

1. Check if MongoDB is accessible from the application:
```bash
docker exec -it barter-app curl http://localhost:3000/api/health
```

2. Access the application in your browser:
```
http://localhost:3000
```

## Step 7: Troubleshooting Common Issues

### If MongoDB Connection Fails:

1. Check MongoDB container status:
```bash
docker ps | grep mongodb
```

2. Check network connectivity:
```bash
docker network inspect my-network
```

3. Verify MongoDB logs:
```bash
docker logs mongodb
```

### If Application Container Fails:

1. Check application logs:
```bash
docker logs barter-app
```

2. Verify environment variables:
```bash
docker exec -it barter-app env | grep MONGODB_URI
```

3. Test MongoDB connection from application container:
```bash
docker exec -it barter-app mongosh mongodb://mongodb:27017/barter-trading
```

## Step 8: Cleanup (When Done)

1. Stop the containers:
```bash
docker stop barter-app mongodb
```

2. Remove the containers:
```bash
docker rm barter-app mongodb
```

3. Remove the network:
```bash
docker network rm my-network
```

4. Remove the image:
```bash
docker rmi thenusan/barter-trading-system
```

## Step 9: Running Tests

1. Run tests in the application container:
```bash
docker exec -it barter-app npm test
```

2. Check test coverage:
```bash
docker exec -it barter-app npm run test:coverage
```

## Step 10: Development Workflow

1. Make changes to your code locally

2. Rebuild the image:
```bash
docker build -t thenusan/barter-trading-system .
```

3. Stop and remove the old container:
```bash
docker stop barter-app
docker rm barter-app
```

4. Start a new container with the updated image:
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://mongodb:27017/barter-trading \
  --name barter-app \
  thenusan/barter-trading-system
```

## Important Notes

1. Always ensure MongoDB container is running before starting the application container
2. Keep the containers on the same network
3. Use the correct container name (mongodb) in the MongoDB URI
4. Check logs if you encounter any issues
5. Remember to clean up resources when you're done

## Next Steps

1. Set up Docker Compose for easier management
2. Configure environment variables
3. Set up volume mounts for persistent data
4. Implement health checks
5. Set up monitoring and logging 