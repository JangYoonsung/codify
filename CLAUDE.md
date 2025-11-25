# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Spotify Insights is a third-party Spotify application providing listening habits analysis, smart playlist generation, and music recommendations. This is a pnpm monorepo managed by Turbo.

## Monorepo Structure

- **apps/backend**: NestJS backend using Domain-Driven Design (DDD) architecture
- **apps/frontend**: Next.js 14 frontend with App Router
- **packages/types**: Shared TypeScript types across the monorepo
- **packages/shared-utils**: Shared utility functions

## Common Commands

### Development
```bash
# Run all apps in development mode
pnpm dev

# Run specific app only
pnpm backend:dev
pnpm frontend:dev
```

### Building
```bash
# Build all apps
pnpm build
```

### Testing
```bash
# Run all tests
pnpm test

# Backend tests
pnpm --filter backend test
pnpm --filter backend test:watch
pnpm --filter backend test:cov
pnpm --filter backend test:e2e
```

### Type Checking & Linting
```bash
# Type check all apps
pnpm type-check

# Lint all apps
pnpm lint

# Backend linting
pnpm --filter backend lint
```

### Cleanup
```bash
# Clean all build artifacts and node_modules
pnpm clean
```

## Backend Architecture (DDD)

The backend follows Domain-Driven Design with a clear layered architecture. Each domain module is organized into four layers:

### Domain Layer Structure

Each domain (user, listening-habits, playlist, recommendation, track) follows this pattern:

```
domain/
├── domain/           # Core business logic
│   ├── entities/     # Domain entities with business rules
│   ├── value-objects/# Immutable value objects
│   └── services/     # Domain services
├── application/      # Use cases and orchestration
│   ├── use-cases/    # Application-level business logic
│   └── services/     # Application services
├── infrastructure/   # External concerns
│   ├── repositories/ # Data persistence implementations
│   └── adapters/     # External service adapters
└── presentation/     # API layer
    ├── controllers/  # HTTP controllers
    └── dtos/         # Data transfer objects
```

### Key Backend Concepts

- **Domain Entities**: Business objects with identity (e.g., User, Playlist, Track)
- **Value Objects**: Immutable objects defined by their values (e.g., EmailAddress, DateRange)
- **Domain Services**: Business logic that doesn't fit in entities
- **Use Cases**: Application-level orchestration of domain logic
- **Repositories**: Abstraction for data persistence (interfaces in domain, implementations in infrastructure)
- **DTOs**: Used only in presentation layer for API input/output

### Shared Backend Infrastructure

Located in `apps/backend/src/`:

- **infrastructure/**: Common infrastructure concerns
  - `database/`: TypeORM configuration and setup
  - `spotify/`: Spotify API client integration
  - `cache/`: Redis cache configuration
  - `auth/`: JWT and Passport authentication setup
- **shared/**: Cross-cutting concerns
  - `guards/`: Route guards (auth, roles)
  - `decorators/`: Custom decorators
  - `filters/`: Exception filters
  - `interceptors/`: Request/response interceptors
  - `pipes/`: Validation pipes
  - `utils/`: Utility functions
- **config/**: Configuration modules

## Frontend Architecture

### Tech Stack
- **Next.js 14**: App Router for file-based routing
- **Zustand**: Lightweight state management
- **TanStack Query**: Server state and data fetching
- **Tailwind CSS**: Utility-first styling
- **Recharts**: Data visualization

### Frontend Structure
```
src/
├── app/          # Next.js App Router (pages and layouts)
├── components/   # React components
├── lib/          # Third-party library configurations
├── hooks/        # Custom React hooks
├── services/     # API service layer (axios)
├── types/        # TypeScript type definitions
└── styles/       # Global styles
```

## Environment Setup

### Prerequisites
- Node.js >= 18
- pnpm >= 8
- PostgreSQL (for backend)
- Redis (for backend caching)
- Spotify Developer Account

### Environment Variables

Backend (`.env`):
- Spotify API credentials (CLIENT_ID, CLIENT_SECRET)
- Database connection (PostgreSQL)
- Redis connection
- JWT secret
- Frontend URL for CORS

Frontend (`.env`):
- `NEXT_PUBLIC_API_URL`: Backend API URL

### Spotify OAuth Setup
- Backend redirect URI: `http://localhost:3001/api/v1/auth/spotify/callback`
- Register at: https://developer.spotify.com/dashboard

## API Documentation

When backend is running in development:
- Swagger UI: http://localhost:3001/api/docs
- API prefix: `/api/v1`

## Development Workflow

1. The backend uses global validation pipes that automatically validate DTOs using class-validator
2. All API responses follow NestJS conventions with proper HTTP status codes
3. Authentication uses JWT with Passport strategies
4. Frontend queries use TanStack Query for caching and state management
5. Database migrations and entities use TypeORM

## Important Notes

- This is an early-stage project - domain modules are not yet fully implemented
- The backend follows strict DDD patterns - keep domain logic isolated from infrastructure
- Always use the shared types package for cross-application type definitions
- Frontend uses App Router conventions (not Pages Router)
- All database access goes through repository abstractions
