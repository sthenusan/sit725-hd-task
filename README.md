# Barter Trading System

## Overview
This project is a barter trading system that allows users to trade items with each other. It includes features for user authentication, item management, trade creation, and more.

## Features
- **User Authentication:** Register, login, and manage user profiles.
- **Item Management:** Create, update, and delete items for trade.
- **Trade Creation:** Propose trades between users with offered and requested items.
- **Trade Status Updates:** Accept or reject trades, which updates the status of the items involved.
- **Messaging:** Users can send messages within a trade to communicate.

## Testing
The project includes comprehensive tests to ensure functionality and reliability.

### Running Tests
To run the tests, use the following command:
```bash
npm test
```

To run the tests serially (recommended to avoid race conditions), use:
```bash
npm test -- --runInBand
```

## Technologies Used
- **Node.js:** Backend runtime environment.
- **Express.js:** Web framework for building the API.
- **MongoDB:** Database for storing user and item data.
- **Mongoose:** ODM for MongoDB.
- **Jest:** Testing framework for unit and integration tests.
- **Cypress:** End-to-end testing framework.

## Getting Started
1. Clone the repository.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Set up your environment variables (e.g., `MONGODB_URI`).
4. Run the application:
   ```bash
   npm run dev
   ```
5. Run the tests:
   ```bash
   npm test
   ```

## Docker Setup and Running Instructions

### Prerequisites
- Docker installed on your system
- Docker Compose (optional, for easier management)

### Step 1: Create Docker Network
First, create a Docker network for container communication:
```bash
docker network create my-network
```

### Step 2: Start MongoDB Container
Start the MongoDB container:
```bash
docker run --network my-network \
  -p 27017:27017 \
  --name mongodb \
  mongo
```

Verify MongoDB is running:
```bash
docker ps | grep mongodb
```

### Step 3: Build Application Image
Build the Docker image for the application:
```bash
docker build -t thenusan/barter-trading-system .
```

### Step 4: Run Application Container
Run the application container with MongoDB connection:
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://mongodb:27017/barter-trading \
  thenusan/barter-trading-system
```

### Step 5: Verify Setup
1. Check if MongoDB is accessible:
```bash
docker exec -it mongodb mongosh --eval "db.runCommand({ ping: 1 })"
```

2. Access the application in your browser:
```
http://localhost:3000
```

### Troubleshooting

#### If MongoDB Connection Fails:
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

#### If Application Container Fails:
1. Check application logs:
```bash
docker logs <container_id>
```

2. Verify environment variables:
```bash
docker exec -it <container_id> env | grep MONGODB_URI
```

### Cleanup
When you're done, you can clean up the containers and network:
```bash
# Stop containers
docker stop mongodb
docker stop <app_container_id>

# Remove containers
docker rm mongodb
docker rm <app_container_id>

# Remove network
docker network rm my-network

# Remove image
docker rmi thenusan/barter-trading-system
```

## Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

## License
This project is licensed under the MIT License.