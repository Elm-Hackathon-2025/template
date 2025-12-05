# Docker Templates

This repository contains Docker templates for various technology stacks and a docker-compose configuration example.

## Available Dockerfile Templates

### 1. Dockerfile.java
Multi-stage Dockerfile for Java applications using Maven and Eclipse Temurin JRE.

**Features:**
- Build stage with Maven 3.9 and JDK 17
- Production stage with JRE 17 Alpine (smaller image)
- Dependency caching for faster builds
- Exposes port 8080

**Usage:**
```bash
docker build -f Dockerfile.java -t my-java-app .
docker run -p 8080:8080 my-java-app
```

### 2. Dockerfile.dotnet
Multi-stage Dockerfile for .NET applications.

**Features:**
- Build stage with .NET SDK 8.0
- Production stage with ASP.NET Core Runtime 8.0
- Optimized layer caching
- Configured to listen on port 8080

**Usage:**
```bash
docker build -f Dockerfile.dotnet -t my-dotnet-app .
docker run -p 8080:8080 my-dotnet-app
```

### 3. Dockerfile.nodejs
Multi-stage Dockerfile for Node.js applications.

**Features:**
- Node.js 20 Alpine base image
- Production dependencies only
- Exposes port 8080
- Environment variable PORT=8080

**Usage:**
```bash
docker build -f Dockerfile.nodejs -t my-nodejs-app .
docker run -p 8080:8080 my-nodejs-app
```

### 4. Dockerfile.react
Multi-stage Dockerfile for React applications with Nginx.

**Features:**
- Build stage with Node.js 20
- Production stage with Nginx Alpine
- Optimized static file serving
- Configured to listen on port 8080

**Usage:**
```bash
docker build -f Dockerfile.react -t my-react-app .
docker run -p 8080:8080 my-react-app
```

## Docker Compose Example

The `docker-compose.yml` file provides a complete example of running a Java application with dependencies.

**Services:**
- **app**: Java application (built from Dockerfile.java)
  - Listens on port 8080
  - Connected to PostgreSQL and Redis
- **postgres**: PostgreSQL 16 database
  - Database: appdb
  - User: appuser
  - Password: apppassword
- **redis**: Redis 7 cache
  - Default port 6379
  - Persistent data volume

**Usage:**
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Stop and remove volumes
docker-compose down -v
```

**Environment Variables:**
The Java app is configured with the following environment variables:
- `SPRING_DATASOURCE_URL`: PostgreSQL connection URL
- `SPRING_DATASOURCE_USERNAME`: Database username
- `SPRING_DATASOURCE_PASSWORD`: Database password
- `SPRING_REDIS_HOST`: Redis host
- `SPRING_REDIS_PORT`: Redis port

**Accessing Services:**
- Application: http://localhost:8080
- PostgreSQL: localhost:5432
- Redis: localhost:6379

## Customization

### Adapting Templates
1. **Java**: Update Maven configuration and artifact name
2. **.NET**: Change the DLL name in ENTRYPOINT
3. **Node.js**: Modify the main file name (default: index.js)
4. **React**: Adjust build output directory if needed

### Docker Compose
You can switch to a different Dockerfile by changing the `dockerfile` field:
```yaml
app:
  build:
    context: .
    dockerfile: Dockerfile.nodejs  # Change to desired Dockerfile
```

### Adding More Dependencies
To add more services (e.g., MongoDB, RabbitMQ):
```yaml
  mongodb:
    image: mongo:7
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network
```

## Best Practices

1. **Multi-stage builds**: All templates use multi-stage builds to keep production images small
2. **Layer caching**: Dependencies are copied before source code for better cache utilization
3. **Health checks**: PostgreSQL and Redis have health checks for proper startup ordering
4. **Persistent volumes**: Data is persisted using Docker volumes
5. **Network isolation**: Services communicate through a dedicated network
6. **Security**: Use environment variables for sensitive data (consider using Docker secrets in production)

## License

This project is licensed under the MIT License - see the LICENSE file for details.