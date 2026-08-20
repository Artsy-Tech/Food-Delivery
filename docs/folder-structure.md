# Detailed Folder Structure

This document outlines the detailed folder structure for the Monolithic Food delivery app architecture.

We have adopted a **Feature-Based (Domain-Driven)** folder structure rather than a purely layered one. For a large-scale application like a food delivery platform, this is considered the best practice. 

**Why this is better:**
When you are working on a feature (like `Orders`), all the relevant files (controller, service, repository, routes, and DTOs) are located in a single `src/order/` folder instead of being scattered across `controllers/`, `services/`, and `models/` folders. This makes the codebase much easier to navigate, maintain, and eventually split into microservices if needed. 

```text
/
├── backend/
│   ├── src/
│   │   ├── users/              # User Domain
│   │   │   ├── users.service.ts
│   │   │   ├── users.controller.ts
│   │   │   ├── users.routes.ts
│   │   │   ├── users.repository.ts
│   │   │   ├── users.entity.ts
│   │   │   └── users.dto.ts
│   │   │
│   │   ├── restaurant/         # Restaurant Domain
│   │   │   ├── restaurant.service.ts
│   │   │   └── ... (controller, routes, repository, entity, dto)
│   │   │
│   │   ├── menu/               # Menu Domain
│   │   ├── cart/               # Cart Domain
│   │   ├── order/              # Order Domain
│   │   ├── payment/            # Payment Domain
│   │   ├── delivery/           # Delivery Domain
│   │   ├── search/             # Search Domain
│   │   ├── reviews/            # Reviews Domain
│   │   ├── notifications/      # Notifications Domain
│   │   ├── auth/               # Authentication Domain
│   │   │
│   │   ├── middleware/         # Shared Express middlewares
│   │   ├── config/             # DB, Redis, Kafka, Env setup
│   │   ├── utils/              # Shared utilities (logger, response formatters)
│   │   │
│   │   ├── app.ts              # Express App setup
│   │   └── server.ts           # Entry point
│   │
│   ├── tests/
│   ├── package.json
│   ├── tsconfig.json
│   ├── Dockerfile
│   └── .env
│
├── frontend/
│   ├── src/
│   │   ├── app/                # Next.js App Router (pages & layouts)
│   │   ├── components/         # Shared UI grouped by domain
│   │   │   ├── navbar/
│   │   │   ├── restaurant/
│   │   │   ├── menu/
│   │   │   ├── cart/
│   │   │   ├── order/
│   │   │   └── common/
│   │   │
│   │   ├── features/           # Feature-sliced Redux/Zustand logic & API calls
│   │   │   ├── auth/
│   │   │   ├── users/
│   │   │   ├── restaurant/
│   │   │   └── ... (menu, cart, order, payment, delivery, search, reviews)
│   │   │
│   │   ├── services/           # Global API clients and WebSockets
│   │   ├── store/              # Root store setup
│   │   ├── hooks/              # Custom React hooks
│   │   ├── types/              # Global TypeScript definitions
│   │   ├── utils/              # Helper functions (formatting, validation)
│   │   ├── constants/          # API routes and app constants
│   │   └── styles/             # Global CSS
│   │
│   ├── public/                 # Static assets
│   ├── tests/
│   ├── package.json
│   ├── tsconfig.json
│   ├── next.config.ts
│   ├── tailwind.config.ts
│   ├── Dockerfile
│   └── .env
│
├── docs/                       # Project Documentation
├── docker-compose.yml          # Orchestrates everything (Frontend, Backend, DB, Redis)
└── package.json                # Root package.json (Husky, root scripts)
```
