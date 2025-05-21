# Barter Trading System

A web application for barter trading built with Node.js, Express, and MongoDB.

## Prerequisites

- Docker
- Docker Compose (optional, for easier management)

## Setup Instructions

### 1. Create Docker Network

First, create a Docker network to allow communication between containers:

```bash
docker network create my-network
```

### 2. Start MongoDB Container

Start the MongoDB container and connect it to the network:

```bash
docker run --network my-network -p 27017:27017 --name mongodb mongo
```

This will:
- Start MongoDB container
- Connect it to `my-network`
- Expose port 27017
- Name the container `mongodb`

### 3. Build the Application Image

Build the Docker image for the application:

```bash
docker build -t thenusan/barter-trading-system .
```

### 4. Run the Application Container

Run the application container with the correct environment variables and network settings:

```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://mongodb:27017/barter-trading \
  thenusan/barter-trading-system
```

This will:
- Map port 3000 to your host machine
- Connect to the same network as MongoDB
- Set the MongoDB connection string
- Start the application

## Troubleshooting

### Port Already in Use

If you see an error about port 3000 being already in use:

1. Check running containers:
```bash
docker ps
```

2. Stop the container using port 3000:
```bash
docker stop <container_id>
```

### MongoDB Connection Issues

If you see "Error connecting to MongoDB: getaddrinfo ENOTFOUND mongodb":

1. Verify MongoDB container is running:
```bash
docker ps | grep mongodb
```

2. Check if containers are on the same network:
```bash
docker network inspect my-network
```

3. Ensure you're using the correct container name in the MongoDB URI:
```bash
docker run -p 3000:3000 \
  --network my-network \
  -e MONGODB_URI=mongodb://<mongodb_container_name>:27017/barter-trading \
  thenusan/barter-trading-system
```

### Testing the Connection

To test if MongoDB is accessible:

```bash
docker exec -it <mongodb_container_name> mongosh --eval "db.runCommand({ ping: 1 })"
```

## Environment Variables

- `MONGODB_URI`: MongoDB connection string (default: mongodb://mongodb:27017/barter-trading)
- `PORT`: Application port (default: 3000)

## Accessing the Application

Once running, access the application at:
```
http://localhost:3000
```

## Development

For local development:

1. Install dependencies:
```bash
npm install
```

2. Start the development server:
```bash
npm run dev
```

## Testing

Run tests with:
```bash
npm test
```

Note: Make sure MongoDB is running and accessible before running tests.

## License

This project is licensed under the MIT License.
