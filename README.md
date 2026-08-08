# 🚀 ResuMaxxing Architecture & System Design

![ResuMaxxing Banner](./assets/banner.png)

> **The Industrial-Grade AI Career Engine.**  
> ResuMaxxing is an automated, AI-driven career operating system designed for high-velocity resume tailoring, job application tracking, and automated document generation.

---

## 📌 Executive Summary & Architecture Overview

ResuMaxxing follows a decoupled **Client-Server & Micro-service Ready Architecture**:
- **Frontend Layer:** Built with **Next.js 15 App Router**, React 19, TypeScript, Vanilla CSS (Design System Tokens), and integrated with **Capacitor** for cross-platform deployment (Web, iOS, Android).
- **Backend Core:** Asynchronous **FastAPI** service operating on Python 3.12, utilizing **SQLAlchemy 2.0 (Async)** and **Alembic** for migrations.
- **Identity & Authentication:** **Clerk Identity Platform** (JWT-based, decoupled RSA JWKS key verification).
- **AI Engine:** Integration with **OpenAI GPT-4o / Chat Completion Models** for structured JSON schema generation and prompt engineering.
- **Database & Storage:** **MySQL / PostgreSQL** relational schema for transactional integrity, storing encrypted profiles, tailored resumes, and job interaction logs.
- **Monetization & Webhooks:** **Lemon Squeezy** payment webhooks with cryptographic HMAC signature verification.

---

## 🏗️ High-Level System Architecture Diagram

```mermaid
graph TD
    subgraph Client Layer
        A[Next.js Web Client]
        B[Mobile App - Capacitor iOS/Android]
    end

    subgraph Authentication & Gateway
        C[Clerk Identity Service]
        D[FastAPI Sentinel Middleware / Security / Limiter]
    end

    subgraph Core Backend Services
        E[User & Profile Router]
        F[Resume Engine & Generator Router]
        G[Job Tracker Router]
        H[Payment & Webhook Router]
    end

    subgraph Data & AI Infrastructure
        I[(Relational Database - MySQL/Postgres)]
        J[OpenAI GPT-4o Engine]
        K[Lemon Squeezy Billing Gateway]
    end

    A -->|HTTPS / REST| D
    B -->|HTTPS / REST| D
    A -.->|JWT Handshake| C
    B -.->|JWT Handshake| C
    D -->|JWKS Verification| C

    D --> E
    D --> F
    D --> G
    D --> H

    E --> I
    F --> I
    G --> I
    H --> I

    F -->|Prompt Engineering & JSON Schema| J
    K -->|Webhook HMAC Events| H
```

---

## 🛠️ Technology Stack Breakdown

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend Framework** | **Next.js 15 (App Router)** | Server & Client Components, SSG/SSR hybrid rendering |
| **Mobile Runtime** | **Capacitor 6** | Native bridging for Android & iOS builds |
| **Styling & UI** | **Vanilla CSS + Lucide Icons** | High-performance CSS custom properties design system |
| **Backend Framework** | **FastAPI (Python 3.12+)** | High-concurrency async ASGI web server |
| **ORM & Database** | **SQLAlchemy 2.0 + Alembic** | Async database access & migrations |
| **Authentication** | **Clerk Auth** | Passwordless, OAuth, JWKS JWT decoding |
| **AI Integration** | **OpenAI API (GPT-4o)** | Automated resume restructuring & score evaluation |
| **Rate Limiting** | **SlowAPI / Redis Ready** | IP and User ID based API rate throttling |
| **Billing Integration**| **Lemon Squeezy API** | Subscription management & webhooks |

---

## 🔒 Security Architecture & Infrastructure Hardening

1. **Decoupled Stateless Auth:**
   - Client requests carry a Bearer JWT issued by Clerk.
   - FastAPI middleware asynchronously fetches and caches JWKS public keys, validating signatures locally without round-trip latency.
2. **Security Headers & CSP:**
   - Strict `Content-Security-Policy` enforcing trusted origins for scripts, frames, and API endpoints.
   - HSTS (`Strict-Transport-Security`), `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`.
3. **Defense-in-Depth Payload Guards:**
   - 10MB payload size restriction on incoming backend HTTP streams to mitigate DoS vectors.
   - Custom exception sentinel shielding internal backtraces from production responses.
4. **Webhook Security:**
   - Cryptographic signature validation (`X-Signature` HMAC SHA-256) on billing and external events.

---

## 🗺️ High-Level API Domain Map

```
/api
 ├── /users
 │    ├── GET  /me               # Fetch user profile & quota
 │    └── PUT  /me               # Update master profile details
 ├── /resumes
 │    ├── GET  /                 # List master & generated resumes
 │    ├── POST /generate         # Trigger AI resume tailoring engine
 │    ├── GET  /{id}             # Fetch resume details & metadata
 │    └── DELETE /{id}           # Archival / soft-delete
 ├── /jobs
 │    ├── GET  /                 # List saved applications
 │    ├── POST /                 # Track new job posting
 │    └── PUT  /{id}             # Update stage (Applied, Interview, Offer)
 └── /webhooks
      └── POST /lemonsqueezy     # Process payment state changes
```

---

## 💡 Architecture Strategy: Public Spec & Private Implementation

### Standard Strategy for Public Technical Overview Repositories
Creating a dedicated public repository for system architecture, RFCs, and API specifications while preserving source code privacy in a separate repository is an **industry-standard practice**.

#### Key Advantages:
1. **IP Protection:** Keeps proprietary core algorithms, AI prompts, business logic, and code assets private.
2. **Public Transparency & Showcase:** Enables developers, investors, and potential clients to evaluate system design, architecture standards, and security without exposing full execution code.
3. **Collaborative Design Reviews:** Facilitates public RFC discussions, design review issues, and documentation updates open to public contributions.

#### Best Practices for Public Architecture Repositories:
- ❌ **Do NOT include:** Private API keys, database credentials, internal service domain names, or secrets (`.env` files).
- ❌ **Do NOT include:** Full raw prompt files containing proprietary IP.
- ✅ **DO include:** High-level diagrams (Mermaid format), data flow diagrams, system requirements, tech stack specs, and general API contracts.

---

*Architected & Developed with precision by [Daikendy](https://github.com/daikendy)*
