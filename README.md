# Food Delivery App

A comprehensive food delivery platform featuring a Next.js frontend, an Express.js backend, and a robust data layer capable of real-time tracking and geospatial queries.

## Documentation
- [Product Requirements Document (PRD)](./docs/prd.md)
- [Comprehensive Feature List](./docs/features.md)
- [Architecture & Tech Stack](./docs/architecture.md)
- [Detailed Folder Structure](./docs/folder-structure.md)
- [Database Schema](./docs/dbschema.md)

## Tech Stack Highlights
- **Frontend**: Next.js, Tailwind CSS
- **Backend**: Node.js, Express.js (Layered Architecture)
- **Database**: PostgreSQL with PostGIS extension
- **Search Engine**: OpenSearch
- **Caching**: Redis
- **Authentication**: JWT, OAuth, bcrypt
- **Payments**: Razorpay
- **Maps**: Google Maps API
- **Infrastructure**: Docker, Nginx, GitHub Actions

## Architecture Setup
This project uses a **Monolithic** directory structure. A single `docker-compose.yml` brings the entire ecosystem to life.

```text
/
├── frontend/                # Next.js Frontend (Feature-Sliced Design)
├── backend/                 # Express.js Backend (Domain-Driven Design)
├── docs/                    # Project documentation
├── package.json             # Root package.json (root scripts, linting, etc.)
└── docker-compose.yml       # Brings up Frontend, Backend, Postgres, Redis, OpenSearch
```

## Getting Started (Coming Soon)
*Instructions for setting up the local development environment using Docker will be provided here once the repository is initialized.*
