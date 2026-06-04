# Internal Company Portal

## 1. Project Description

The **Internal Company Portal** is a Proof of Concept (PoC) developed for **Nexus Advisory Group Ltd.** to improve internal communication, employee onboarding, and department-level collaboration.

The platform enables structured content management with strong **Role-Based Access Control (RBAC)**, allowing employees to access and manage information according to their role and department.

**Key Objectives:**
- Centralize internal information
- Improve new employee onboarding
- Enable controlled publishing and department-scoped visibility
- Validate a clean, maintainable architecture (Hexagonal + Headless CMS)

---

## 2. Tech Stack

### Core
- **NestJS** (Node.js + TypeScript) – Application Layer
- **Directus** – Headless CMS & RBAC engine (on PostgreSQL)
- **React Admin** – Administrative frontend

### Infrastructure & Data
- **PostgreSQL** – Primary database
- **Docker & Docker Compose** – Containerization
- **Zod** – Runtime API contract validation

### External Services
- **Azure AD** – Authentication
- **SendGrid** – Email & OTP delivery
- **OneDrive** – Archiving (future)

---

## 3. Architecture Overview

The project follows a **Hexagonal Architecture (Ports & Adapters)** to ensure:

- Clear separation between business logic and technical implementation
- High testability
- Future flexibility (easy to replace Directus or other tools)

### Key Layers
- **Domain Layer** — Core business rules and entities
- **Application Layer** — Use case orchestration (NestJS)
- **Ports** — Interfaces defining contracts
- **Adapters** — Implementations for Directus, Azure AD, SendGrid, etc.
- **Inbound Adapters** — React Admin + API layer

> **Important Rule**: The frontend (**React Admin**) **must never** access Directus directly. All interactions go through the Application Layer.

---

## 4. Project Structure

```bash
internal-company-portal/
├── backend/                  # NestJS Application (Hexagonal)
│   ├── src/
│   │   ├── domain/           # Business entities & rules
│   │   ├── application/      # Use cases & services
│   │   ├── ports/            # Interfaces
│   │   ├── adapters/         # Directus, SendGrid, Azure AD...
│   │   └── infrastructure/   # Config, database, providers
│   └── test/
├── frontend/                 # React Admin
│   ├── src/
│   │   ├── resources/
│   │   ├── providers/        # Custom data provider
│   │   └── components/
├── directus/                 # Directus config & extensions
├── docker/                   # Docker configuration
├── adr/                      # Architecture Decision Records
├── docs/                     # Technical documentation
└── README.md

``
---

## 5. Prerequisites

Make sure the following tools are installed:

- **Node.js (>= 18)**
- **Docker & Docker Compose**
- **Git**
- **npm**

Optional:
- **Postman** (for API testing)

---

## 6. Installation & Setup

### Step 1: Clone the repository

git clone https://github.com/Robel-Manoa/internal-company-portal.git
cd internal-company-portal

### Step 2: Create environment files
Create .env files for:

cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env
cp directus/.env.example directus/.env

### Step 3: Configure environment variables
See section Environment Variables below

### Step 4: Install dependencies
cd backend && npm install
cd ../frontend && npm install

## 7. Running the Project
Run with Docker: docker-compose up --build

Services started:

- Backend (NestJS)
- Frontend (React Admin)
- Directus
- PostgreSQL

## 8. Environment Variables
Backend (backend/.env)

PORT=3000
DATABASE_URL=postgres://user:password@db:5432/portal
DIRECTUS_URL=http://directus:8055
DIRECTUS_TOKEN=your-token

AZURE_AD_CLIENT_ID=...
AZURE_AD_TENANT_ID=...

SENDGRID_API_KEY=...

JWT_SECRET=...


Frontend (frontend/.env)
VITE_API_URL=http://localhost:3000/api

Directus (directus/.env)
KEY=your-secret-key
SECRET=your-secret
DB_CLIENT=pg
DB_HOST=db
DB_PORT=5432
DB_DATABASE=portal
DB_USER=user
DB_PASSWORD=password

## 9. Key Features Implemented

- Role-Based Access Control (RBAC)
- Department-level isolation
- Announcements management
- Feedback system with user ownership
- Audit-friendly operations
- OTP authentication via email
- Microsoft ecosystem integration readiness
- Structured API contracts using Zod

## 10. Development Guidelines
### Architecture Rules

- Always follow Hexagonal Architecture
- Business logic must remain in Domain/Application layers
- External services must be accessed via Adapters
- Never couple business logic to Directus, UI, or external tools

### Anti-Patterns to Avoid

- Direct frontend → Directus API calls
- Business logic inside controllers or UI
- Bypassing validation (Zod schemas)

### Code Practices

- Use TypeScript strictly
- Keep modules small and focused
- Prefer interfaces (ports) over concrete implementations
- Write testable use cases

