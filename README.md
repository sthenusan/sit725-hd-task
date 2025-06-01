# Barter Trading System

## Overview
A barter trading system that enables users to trade items with each other. The system includes user authentication, item management, trade creation, and messaging features.

## Features
- User Authentication (Register, Login, Profile Management)
- Item Management (Create, Update, Delete Items)
- Trade Creation and Management
- Real-time Messaging within Trades
- Trade Status Updates
- User Ratings and Reviews

## Technologies Used
- Node.js & Express.js
- MongoDB & Mongoose
- Docker
- Jest & Cypress for Testing

## Docker Setup and Running Instructions

### Prerequisites
- Docker installed on your system
- Docker Compose (optional)

### Step 1: Create Docker Network
Create a Docker network for container communication:
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

### Step 5: Student API
The application includes a simple student API endpoint that returns student information:

```bash
# Access the student API endpoint
curl http://localhost:3000/api/student
```

Expected response:
```json
{
    "name": "Thenusan Santhirakumar",
    "studentId": "S223228828"
}
```

### Step 6: Verify Setup
1. Check MongoDB connection:
```bash
docker exec -it mongodb mongosh --eval "db.runCommand({ ping: 1 })"
```

2. Access the main application:
```
http://localhost:3000
```

3. Test the student API:
```
http://localhost:3000/api/student
```

### Troubleshooting

#### MongoDB Connection Issues
1. Check MongoDB container:
```bash
docker ps | grep mongodb
```

2. Check network:
```bash
docker network inspect my-network
```

3. View MongoDB logs:
```bash
docker logs mongodb
```

#### Application Issues
1. Check application logs:
```bash
docker logs <container_id>
```

2. Verify environment variables:
```bash
docker exec -it <container_id> env | grep MONGODB_URI
```

### Cleanup
To clean up Docker resources:
```bash
# Stop and remove containers
docker stop mongodb
docker stop <app_container_id>
docker rm mongodb
docker rm <app_container_id>

# Remove network
docker network rm my-network

# Remove image
docker rmi thenusan/barter-trading-system
```

## Testing
Run tests using:
```bash
npm test
```

For serial test execution (recommended):
```bash
npm test -- --runInBand
```

## Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

## License
This project is licensed under the MIT License.
