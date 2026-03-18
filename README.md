# Golang API Boilerplate Template

[![Go](https://img.shields.io/badge/Go-1.25+-00ADD8?style=flat&logo=go&logoColor=white)](https://golang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Go Report Card](https://goreportcard.com/badge/github.com/faytranevozter/go-template)](https://goreportcard.com/report/github.com/faytranevozter/go-template)

> A production-ready boilerplate for building RESTful APIs with Clean Architecture in Go

## 🎯 Overview

This is a comprehensive boilerplate template for building scalable RESTful APIs in [Golang](https://golang.org/). It implements [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) principles, providing a solid foundation for your next microservice or API project.

The architecture is inspired by [go-clean-arch](https://github.com/bxcodec/go-clean-arch) with extensive customizations and modern best practices. This template helps you get started quickly with well-organized code structure and industry-standard patterns.

## ✨ Features

- 🏗️ **Clean Architecture** — Separation of concerns with clear boundaries between layers
- 🔐 **JWT Authentication** — Built-in JWT token generation and validation
- 🗄️ **Multiple Database Support** — MySQL, PostgreSQL, SQLite, SQL Server, and MongoDB
- 💾 **Redis Caching** — Integrated caching layer with middleware
- ☁️ **AWS S3 Integration** — File storage and upload support
- 🚀 **RESTful API** — Built with [Gin](https://github.com/gin-gonic/gin) framework for high performance
- 📝 **Structured Logging** — Request/response logging with [Logrus](https://github.com/sirupsen/logrus) and log rotation
- 🔄 **CORS Support** — Configurable cross-origin resource sharing
- 🛡️ **Middleware Suite** — Auth, cache, CORS, logger, and recovery
- 🐳 **Docker & Compose** — Dockerfile and Docker Compose for easy containerization
- 📊 **Data Export** — Export data to Excel with built-in support

## 📋 Tech Stack

| Category | Technology |
|----------|-----------|
| **Language** | [Go](https://golang.org/) 1.25+ |
| **Web Framework** | [Gin](https://github.com/gin-gonic/gin) |
| **ORM** | [GORM](https://gorm.io/) |
| **SQL Databases** | MySQL, PostgreSQL, SQLite, SQL Server |
| **NoSQL Database** | [MongoDB](https://www.mongodb.com/) 6.0+ |
| **Cache** | [Redis](https://redis.io/) |
| **Authentication** | [JWT](https://jwt.io/) via [golang-jwt](https://github.com/golang-jwt/jwt) |
| **Cloud Storage** | [AWS S3](https://aws.amazon.com/s3/) |
| **Logging** | [Logrus](https://github.com/sirupsen/logrus) + [Lumberjack](https://github.com/natefinsh/lumberjack) |

## 📐 Architecture

This project follows **Clean Architecture** principles. Each layer has clear responsibilities and dependencies only point inward:

```
┌──────────────────────────────────────────────────┐
│                 Frameworks & Drivers              │
│  (Gin, GORM, MongoDB Driver, Redis, AWS SDK)      │
├──────────────────────────────────────────────────┤
│              Interface Adapters                   │
│  (REST Handlers, Middleware, Repositories)         │
├──────────────────────────────────────────────────┤
│              Application Business Rules           │
│  (Services / Use Cases)                           │
├──────────────────────────────────────────────────┤
│              Enterprise Business Rules            │
│  (Domain Entities, Interfaces)                    │
└──────────────────────────────────────────────────┘
```

## 📁 Project Structure

```
.
├── app/                        # Application entry point
│   └── main.go                 #   Bootstrap and wire up dependencies
├── domain/                     # Enterprise Business Rules (entities & interfaces)
│   ├── user.go                 #   User entity and repository interface
│   ├── article.go              #   Article entity (MongoDB example)
│   ├── error.go                #   Error code definitions
│   └── model/                  #   Data transfer objects (DTOs)
│       ├── auth/               #     JWT claim structures
│       ├── request/            #     Request payloads
│       ├── response/           #     Response structures
│       ├── filter.go           #     Query filter models
│       ├── gorm/               #     GORM query helpers
│       ├── mongo/              #     MongoDB query helpers
│       └── storage/            #     Storage response models
├── helpers/                    # Shared utility functions
│   ├── common.go               #   General helpers (JSON, email validation)
│   ├── pagination.go           #   Pagination helpers (limit/offset)
│   ├── connection/             #   Database connection factories
│   │   ├── mysql.go            #     MySQL connection
│   │   ├── postgres.go         #     PostgreSQL connection
│   │   ├── sqlite.go           #     SQLite connection
│   │   ├── sqlserver.go        #     SQL Server connection
│   │   ├── mongodb.go          #     MongoDB connection
│   │   └── redis.go            #     Redis connection
│   └── jsonwebtoken/           #   JWT utilities
│       ├── init.go             #     JWT initialization
│       └── secret.go           #     Secret key management
├── internal/                   # Private application code
│   ├── repository/             #   Data access layer implementations
│   │   ├── gorm/               #     SQL repositories (via GORM)
│   │   ├── mongo/              #     MongoDB repositories
│   │   ├── redis/              #     Redis cache operations
│   │   └── s3/                 #     AWS S3 file storage
│   ├── rest/                   #   HTTP transport layer
│   │   ├── user.go             #     User route handlers
│   │   └── middleware/         #     HTTP middlewares
│   │       ├── auth.go         #       JWT authentication
│   │       ├── cache.go        #       Redis response caching
│   │       ├── cors.go         #       CORS configuration
│   │       ├── logger.go       #       Request/response logging
│   │       └── recovery.go     #       Panic recovery
│   └── worker/                 #   Background workers / consumers
├── user/                       # User module (example use case)
│   ├── service.go              #   Business logic / use cases
│   ├── storage.go              #   Storage interface definition
│   └── cache.go                #   Cache interface definition
├── docker-compose.yml          # Docker Compose for local development
├── Dockerfile                  # Multi-stage Docker build
├── Makefile                    # Build automation commands
├── .env.example                # Environment variable template
└── go.mod                      # Go module dependencies
```

## 🚀 Getting Started

### Prerequisites

- [Go](https://golang.org/dl/) 1.25 or higher
- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) (for containerized setup)
- Or manually install: MySQL/PostgreSQL/SQLite + Redis (optional) + AWS S3 credentials (optional)

### Quick Start with Docker Compose

The fastest way to get the full stack running locally:

```bash
# 1. Clone the repository
git clone https://github.com/faytranevozter/go-template.git
cd go-template

# 2. Copy and configure environment variables
cp .env.example .env

# 3. Start all services (app + MySQL + Redis)
docker compose up -d

# 4. Verify the app is running
curl http://localhost:5050/
# {"message":"It works"}
```

To stop all services:
```bash
docker compose down
```

To stop and remove all data volumes:
```bash
docker compose down -v
```

### Manual Setup (without Docker)

1. **Clone this repository**
   ```bash
   git clone https://github.com/faytranevozter/go-template.git
   cd go-template
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   ```

3. **Configure your `.env` file**

   Edit the `.env` file with your database credentials, Redis configuration, AWS credentials, and other settings. See [Environment Variables](#-environment-variables) for details.

4. **Run the application**
   ```bash
   make run
   ```

   Or using Go directly:
   ```bash
   go run app/main.go
   ```

The application will download all dependencies automatically and start the server on port `5050`.

## 🛠️ Available Commands

| Command | Description |
|---------|-------------|
| `make run` | Run the application |
| `make build` | Build optimized binary (output: `build-app`) |
| `make test` | Run all tests with coverage |
| `make testcoverage` | Run tests and generate coverage report |
| `make generate-mocks` | Generate mock files for testing |

## 🌐 API Endpoints

### Public Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Health check |
| `POST` | `/auth/login` | User login |
| `POST` | `/auth/register` | User registration |

### Protected Endpoints (require JWT Bearer token)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/auth/me` | Get current authenticated user |
| `GET` | `/sample/user/list` | List users (supports pagination & filters) |
| `GET` | `/sample/user/detail/:id` | Get user by ID |
| `GET` | `/sample/user/export` | Export users to Excel |

### Authentication

Include the JWT token in the `Authorization` header:

```
Authorization: Bearer <your-jwt-token>
```

**Login example:**

```bash
curl -X POST http://localhost:5050/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "your-password"}'
```

**Register example:**

```bash
curl -X POST http://localhost:5050/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com", "password": "secure-password"}'
```

**Access protected endpoint:**

```bash
curl http://localhost:5050/auth/me \
  -H "Authorization: Bearer <your-jwt-token>"
```

## 🐳 Docker

### Docker Compose (Recommended)

The included `docker-compose.yml` sets up the complete development environment:

| Service | Description | Port |
|---------|-------------|------|
| `app` | Go API application | `5050` |
| `mysql` | MySQL 8.0 database | `3306` |
| `redis` | Redis 7 cache | `6379` |

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f app

# Rebuild after code changes
docker compose up -d --build app

# Stop all services
docker compose down
```

### Standalone Docker

Build and run the Docker image directly:

```bash
# Build
docker build -t build-app .

# Run with env file
docker run -p 5050:5050 --env-file .env -d build-app

# Run with inline env vars
docker run -p 5050:5050 \
  -e DB_URL="mysql://user:password@host:3306/dbname" \
  -e GO_ENV=production \
  -d build-app
```

## ⚙️ Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Server port | `5050` |
| `GO_ENV` | Environment (`production` or `development`) | — |
| `TIMEOUT` | Request timeout in seconds | `5` |
| **Logging** | | |
| `LOG_TO_STDOUT` | Log to standard output | `true` |
| `LOG_TO_FILE` | Log to file | `false` |
| `LOG_FILENAME` | Log file path | `server.log` |
| `LOG_MAX_SIZE` | Max log file size in MB | `50` |
| **Database** | | |
| `DB_URL` | Database connection URL | — |
| **Redis** | | |
| `USE_REDIS` | Enable Redis caching | `false` |
| `REDIS_URL` | Redis connection URL | `redis://localhost:6379/0` |
| `REDIS_TTL` | Cache TTL | `60s` |
| `REDIS_KEY_PREFIX` | Cache key prefix | `app:` |
| **JWT** | | |
| `JWT_MEMBER_SECRET_KEY` | JWT signing secret | — |
| `JWT_MEMBER_TTL` | Token TTL in minutes | `60` |
| **AWS S3** | | |
| `S3_ENDPOINT` | S3 endpoint URL | `https://s3.amazonaws.com` |
| `S3_REGION` | S3 region | `auto` |
| `S3_ACCESS_KEY` | S3 access key | — |
| `S3_SECRET_KEY` | S3 secret key | — |
| `S3_BUCKET_NAME` | S3 bucket name | — |
| `S3_PUBLIC_URL` | Public URL for S3 assets | — |

## 📖 Usage Guide

### Adding a New Module

Follow the Clean Architecture pattern to add new modules:

1. **Define the domain entity** in `domain/` — create the struct and repository interface
2. **Create the service** in a new module directory — implement business logic
3. **Implement the repository** in `internal/repository/` — data access layer
4. **Create REST handlers** in `internal/rest/` — HTTP transport layer
5. **Wire it up** in `app/main.go` — dependency injection

**Example: Adding an "Order" module**

```
domain/order.go              # Order entity + OrderRepository interface
order/service.go             # Order business logic
order/storage.go             # Storage interface
internal/repository/gorm/order.go   # GORM implementation
internal/rest/order.go       # REST handlers
```

### Switching Databases

To switch the SQL database driver, edit `app/main.go` and uncomment the desired connection:

```go
gormDB, err := gorm.Open(
    // connection.NewMysqlGORM(timeoutContext, os.Getenv("DB_URL")),       // MySQL
    // connection.NewPostgresGORM(timeoutContext, os.Getenv("DB_URL")),    // PostgreSQL
    // connection.NewSQLiteGORM(timeoutContext, os.Getenv("DB_URL")),     // SQLite
    // connection.NewSQLServerGORM(timeoutContext, os.Getenv("DB_URL")),  // SQL Server
    &gorm.Config{},
)
```

### Using Redis Caching Middleware

Apply the cache middleware to any route:

```go
router.GET("/data", mdl.Cache(), handler.GetData)
```

### Using JWT Authentication Middleware

Protect routes with the auth middleware:

```go
router.GET("/profile", mdl.Auth(), handler.GetProfile)
```

## 🧪 Testing

Run the test suite:

```bash
# Run all tests
make test

# Run tests with coverage report
make testcoverage

# Run specific package tests
go test -v ./user/...
go test -v ./internal/rest/...
```

## 📝 License

This project is licensed under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to open a pull request or submit an issue.

## 📧 Contact

Fahrur Rifai — [mfahrurrifai@gmail.com](mailto:mfahrurrifai@gmail.com)

---

**Happy coding! 🚀**