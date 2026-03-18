# AI Agent Instructions

This document provides context and guidelines for AI coding assistants working on this codebase.

## Project Overview

This is a Go RESTful API boilerplate following **Clean Architecture** principles. It uses the [Gin](https://github.com/gin-gonic/gin) web framework with GORM for SQL databases, MongoDB driver for NoSQL, Redis for caching, and JWT for authentication.

**Module name:** `app` (defined in `go.mod`)
**Go version:** 1.25+
**Default port:** 5050

## Architecture

The project follows Clean Architecture with these layers (dependencies point inward):

1. **Domain Layer** (`domain/`) — Entities, repository interfaces, error codes, and DTOs
2. **Use Case Layer** (`user/`, and other module directories) — Business logic services
3. **Interface Adapters** (`internal/rest/`, `internal/repository/`) — HTTP handlers, middleware, and data access implementations
4. **Frameworks & Drivers** (`helpers/connection/`, `app/main.go`) — Database drivers, external SDKs, and bootstrap

### Key Conventions

- **Domain entities** define repository interfaces in the same file (e.g., `domain/user.go` defines `UserRepository`)
- **Services** accept repository interfaces via constructor injection (e.g., `user.NewService(repo)`)
- **REST handlers** define their own service interface locally (e.g., `internal/rest/user.go` defines `UserService`)
- **Dependency wiring** happens in `app/main.go`
- **Middleware** is initialized via `middleware.NewMiddleware()` and methods are called on the instance

## Directory Layout

```
app/                    → Entry point (main.go with DI wiring)
domain/                 → Business entities and interfaces
domain/model/           → DTOs: auth, request, response, filter, storage
helpers/                → Shared utilities (pagination, email validation, JSON)
helpers/connection/     → Database/cache connection factories
helpers/jsonwebtoken/   → JWT token utilities
internal/repository/    → Repository implementations (gorm, mongo, redis, s3)
internal/rest/          → HTTP handlers
internal/rest/middleware/ → Middleware (auth, cache, cors, logger, recovery)
internal/worker/        → Background workers / consumers
user/                   → User module (service, storage interface, cache interface)
```

## Build & Test Commands

```bash
# Run the application
make run
# or: go run app/main.go

# Build optimized binary
make build
# or: go build -ldflags="-s -w" -o build-app app/main.go

# Run all tests with coverage
make test
# or: go test -v -cover -covermode=atomic ./...

# Run tests with coverage report
make testcoverage

# Generate mocks
make generate-mocks

# Docker build
docker build -t build-app .

# Docker Compose (full stack)
docker compose up -d
```

## Testing Practices

- Tests use Go's standard `testing` package — **no external test frameworks** (no testify)
- Mock implementations are created manually by implementing interfaces inline in test files
- Test files are co-located with source files (e.g., `user/service_test.go`)
- Run tests with: `go test -v -cover ./...`
- Naming convention: `TestFunctionName` or `TestFunctionName_scenario`

## Code Style & Conventions

- **Error handling**: Use `domain.ErrCode*` constants for error codes
- **Response format**: Use `response.Success()`, `response.Error()`, `response.SuccessList()` helpers from `domain/model/response/`
- **HTTP status codes**: Return `(statusCode int, response response.Base)` tuples from service methods
- **Environment variables**: Loaded via `godotenv` from `.env` file; see `.env.example` for all options
- **Logging**: Use `logrus` for structured JSON logging
- **Database URLs**: Use connection string format (e.g., `mysql://user:password@host:3306/dbname`)
- **Pagination**: Use `helpers.GetLimitOffset(query)` to extract limit/offset from query parameters
- **Context**: Pass `context.Context` through all service and repository methods

## API Routes

Public:
- `GET  /`                    → Health check
- `POST /auth/login`         → Login (returns JWT token)
- `POST /auth/register`      → Register new user

Protected (require `Authorization: Bearer <token>` header):
- `GET /auth/me`              → Get authenticated user info
- `GET /sample/user/list`     → List users (pagination + filters)
- `GET /sample/user/detail/:id` → Get user by ID
- `GET /sample/user/export`   → Export users to Excel

## Adding New Modules

1. Define entity struct and repository interface in `domain/`
2. Create module directory (e.g., `order/`)
3. Add `service.go` with business logic, accepting repository interface
4. Add `storage.go` with storage interface definition
5. Implement repository in `internal/repository/gorm/` (or `mongo/`)
6. Create REST handler in `internal/rest/` with local service interface
7. Wire dependencies in `app/main.go`

## Docker & Infrastructure

- **Dockerfile**: Multi-stage build (golang:1.25 builder → alpine runtime)
- **docker-compose.yml**: App + MySQL 8.0 + Redis 7
- **Port mapping**: App on 5050, MySQL on 3306, Redis on 6379
- The app uses `DB_URL` env var for database connection
- Redis is optional — controlled by `USE_REDIS` env var

## Important Notes

- The `internal/` directory is Go's convention for private packages — not importable outside this module
- GORM auto-migration or manual schema setup is expected by the consumer of this template
- The default database driver is MySQL; switch by uncommenting the desired driver in `app/main.go`
- JWT secret key (`JWT_MEMBER_SECRET_KEY`) must be set for authentication to work
- S3 configuration is optional and only needed if file upload features are used
