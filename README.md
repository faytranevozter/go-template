# Golang API Boilerplate Template

> A production-ready boilerplate for building RESTful APIs with Clean Architecture in Go

## 🎯 Overview

This is a comprehensive boilerplate template for building scalable RESTful APIs in [Golang](https://golang.org/). It implements [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) principles, providing a solid foundation for your next microservice or API project.

The architecture is inspired by [go-clean-arch](https://github.com/bxcodec/go-clean-arch) with extensive customizations and modern best practices. This template helps you get started quickly with well-organized code structure and industry-standard patterns.

## ✨ Features

- 🏗️ **Clean Architecture** - Separation of concerns with clear boundaries between layers
- 🔐 **JWT Authentication** - Built-in JWT token generation and validation
- 🗄️ **Multiple Database Support** - MySQL, PostgreSQL, SQLite, and MongoDB
- 💾 **Redis Caching** - Integrated caching layer with middleware
- ☁️ **AWS S3 Integration** - File storage and upload support
- 🚀 **RESTful API** - Built with Gin framework for high performance
- 📝 **Structured Logging** - Request/response logging with Logrus
- 🔄 **CORS Support** - Configurable cross-origin resource sharing
- 🛡️ **Middleware Suite** - Auth, cache, CORS, logger, and recovery
- 🐳 **Docker Ready** - Dockerfile included for containerization

## 📋 Tech Stack

### Core
- **Go** 1.25+
- **[Gin](https://github.com/gin-gonic/gin)** - HTTP web framework
- **[Logrus](https://github.com/sirupsen/logrus)** - Structured logger

### Databases
- **[MongoDB](https://www.mongodb.com/)** 6.0+ - NoSQL database
- **[MySQL](https://www.mysql.com/)** - Relational database
- **[PostgreSQL](https://www.postgresql.org/)** - Advanced relational database
- **[SQLite](https://www.sqlite.org/)** - Embedded database
- **[GORM](https://gorm.io/)** - ORM library

### Infrastructure
- **[Redis](https://redis.io/)** - In-memory cache
- **[AWS S3](https://aws.amazon.com/s3/)** - Cloud storage
- **[JWT](https://jwt.io/)** - Token-based authentication

## 📁 Project Structure

```
.
├── app/                    # Application entry point
│   └── main.go
├── domain/                 # Business entities and interfaces
│   ├── article.go
│   ├── user.go
│   ├── error.go
│   └── model/             # Data models and DTOs
├── helpers/               # Utility functions and helpers
│   ├── connection/        # Database connections
│   ├── jsonwebtoken/      # JWT utilities
│   ├── common.go
│   └── pagination.go
├── internal/              # Private application code
│   ├── repository/        # Data access layer
│   │   ├── gorm/         # SQL repositories
│   │   ├── mongo/        # MongoDB repositories
│   │   ├── redis/        # Cache repositories
│   │   └── s3/           # Storage repositories
│   ├── rest/             # HTTP handlers
│   │   └── middleware/   # HTTP middlewares
│   └── worker/           # Background workers
├── user/                 # User module (example)
│   ├── service.go        # Business logic
│   ├── storage.go        # Storage interface
│   └── cache.go          # Cache layer
├── Dockerfile            # Docker configuration
├── Makefile             # Build automation
└── go.mod               # Go dependencies
```

## 🚀 Getting Started

### Prerequisites

- Go 1.25 or higher
- One or more databases (MySQL/PostgreSQL/MongoDB/SQLite)
- Redis (optional, for caching)
- AWS S3 credentials (optional, for file storage)

### Installation

1. **Clone this repository**
   ```bash
   git clone <repository-url>
   cd <project-directory>
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   ```
   
3. **Configure your `.env` file**
   
   Edit the `.env` file with your database credentials, Redis configuration, AWS credentials, and other settings.

4. **Run the application**
   ```bash
   make run
   ```
   
   Or using Go directly:
   ```bash
   go run app/main.go
   ```

The application will download all dependencies automatically and start the server.

## 🛠️ Available Commands

### Development
```bash
make run              # Run the application
make test             # Run all tests with coverage
make testcoverage     # Run tests and generate coverage report
make generate-mocks   # Generate mock files for testing
```

### Build
```bash
make build           # Build optimized binary (output: build-app)
```

## 🐳 Docker Deployment

### Build Docker Image
```bash
docker build -t build-app . && docker image prune -f
```

### Run Docker Container
```bash
docker run -p 5050:5050 --env-file .env -d build-app
```

Or pass environment variables directly:
```bash
docker run -p 5050:5050 \
  -e DB_HOST=localhost \
  -e DB_PORT=3306 \
  -d build-app
```

For more options, see [Docker environment variables documentation](https://docs.docker.com/engine/reference/commandline/run/#set-environment-variables--e---env---env-file).

## 📖 Usage Guide

### Adding a New Module

1. **Define domain entity** in `domain/`
2. **Create storage interface** in your module directory
3. **Implement repository** in `internal/repository/`
4. **Add business logic** in service layer
5. **Create REST handlers** in `internal/rest/`
6. **Register routes** in your router configuration

### Example module structure (User):
```
user/
├── service.go    # Business logic and use cases
├── storage.go    # Storage interface definition
└── cache.go      # Caching layer
```

## 🔒 Authentication

JWT authentication is built-in. Use the `auth` middleware to protect your routes:

```go
router.Use(middleware.Auth())
```

Token generation utilities are available in `helpers/jsonwebtoken/`.

## 🗄️ Database Support

This boilerplate supports multiple databases simultaneously:

- **SQL databases** via GORM (MySQL, PostgreSQL, SQLite)
- **NoSQL** via MongoDB driver
- **Caching** via Redis

Connection helpers are in `helpers/connection/`.

## 📝 License

MIT License

Copyright (c) 2025 Fahrur Rifai

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

## 📧 Contact

You can reach me at [mfahrurrifai@gmail.com](mailto:mfahrurrifai@gmail.com).

---

**Happy coding! 🚀**