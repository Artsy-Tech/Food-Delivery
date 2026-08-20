# Architecture & Technical Stack

This document outlines the architecture and technical stack for the Food delivery app.

## 1. High-Level Architecture
The system follows a **Monolithic** architecture. Instead of splitting into a complex monorepo, everything lives in a single repository with a unified setup. A single `docker-compose.yml` at the root will bring the entire stack to life (Frontend, Backend, Database, Caching, and Search).

- **Root Structure**: The root directory contains the `docker-compose.yml` and a root `package.json` (for managing root-level scripts or dependencies). 
- **Frontend & Backend**: The codebase will contain separate directories for the Next.js client and the Express.js server, but they will be orchestrated together via Docker.
- **Caching Layer**: In-memory data store for frequently accessed data (menus, sessions).
- **Search Engine**: Dedicated search infrastructure for complex text and geospatial queries.
- **Database**: Relational database for structured data with spatial extensions.

## 2. Technical Stack

### Frontend
- **Framework**: Next.js (React)
- **Styling**: Tailwind CSS
- **State Management**: Zustand / React Query (Recommended for API state)
- **Maps**: Google Maps API (for location selection, order tracking)

### Backend
- **Framework**: Express.js (Node.js)
- **Architecture**: Layered Service Architecture (Routes -> Controllers -> Services -> Repositories)
- **Authentication**: JWT (JSON Web Tokens) for session management, OAuth for social logins (Google, Apple).
- **Security**: bcrypt for password hashing.
- **Payments**: Razorpay Gateway Integration.

### Data & Infrastructure
- **Primary Database**: PostgreSQL
- **Geospatial Extension**: PostGIS (for location-based restaurant discovery and delivery routing)
- **Caching**: Redis (for caching restaurant menus, OTPs, user sessions, and rate limiting)
- **Search**: OpenSearch (for fast search across restaurants, cuisines, and dishes with typo tolerance and autocomplete)
- **Containerization**: Docker & Docker Compose
- **Reverse Proxy / Web Server**: Nginx
- **CI/CD**: GitHub Actions

## 3. Backend Layered Architecture Details

The `api` service will be structured strictly into the following layers to ensure separation of concerns:

1. **Routes**: Defines API endpoints and maps them to controllers. Handles basic request validation.
2. **Controllers**: Extracts parameters and body from the request, calls the appropriate service, and formats the HTTP response.
3. **Services**: Contains the core business logic. This layer orchestrates data fetching, processing, and external API calls (e.g., Razorpay, Google Maps).
4. **Repositories**: The only layer that interacts with the database. Contains SQL queries or ORM methods.
5. **Models**: Defines the data structures and relationships.

## 4. Key Integration Points
- **Payment Flow**: 
  - User initiates checkout -> Backend creates Razorpay order -> Frontend opens Razorpay checkout -> Razorpay webhook notifies backend of success -> Backend updates order status.
- **Location & Search Flow**: 
  - User searches "Pizza near me" -> Frontend gets user coordinates via browser or Google Maps API -> Backend queries OpenSearch/PostGIS -> Returns sorted results by distance and relevance.
- **Order Tracking Flow**:
  - Delivery partner app updates location via API -> Backend updates Redis cache -> Frontend polls or uses WebSockets to get live location -> Rendered on Google Maps.

## 5. Deployment Architecture
- **Docker**: The entire stack (Frontend, Backend, Postgres, Redis, OpenSearch, Nginx) will be containerized. `docker-compose.yml` will be used for local development.
- **Nginx**: Acts as an API Gateway/Reverse Proxy to route requests to the Next.js frontend or Express.js backend, and handle SSL termination.
- **GitHub Actions**: Automated pipelines for linting, testing, and building Docker images upon code push.
