# Multi-vendor E-commerce Platform

Backend marketplace nhiều nhà bán hàng, xây dựng bằng **Java + Spring Boot** theo kiến trúc **Modular Monolith**.

## Project Goal

Xây dựng một nền tảng thương mại điện tử cho phép nhiều vendor cùng bán hàng, tập trung vào các nghiệp vụ backend thực tế: RBAC, checkout đa vendor, inventory concurrency, payment idempotency, commission ledger, testing và CI/CD.

## MVP Scope

- Authentication & Authorization
- Vendor registration and approval
- Category / Product / Variant catalog
- Cart
- Inventory & reservation
- Multi-vendor checkout
- Order / Sub-order lifecycle
- COD + payment sandbox
- Commission & payout ledger
- Email notification
- Admin reports

Các tính năng như review/rating, wishlist, coupon phức tạp, chat và recommendation được để cho giai đoạn mở rộng.

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 17 / 21 |
| Framework | Spring Boot 3.x |
| Security | Spring Security, JWT |
| Database | PostgreSQL |
| Persistence | Spring Data JPA / Hibernate |
| Migration | Flyway |
| Cache / Lock | Redis |
| API Docs | Springdoc OpenAPI / Swagger |
| Mapping | MapStruct, Lombok |
| Testing | JUnit 5, Mockito, Testcontainers, RestAssured |
| DevOps | Docker, Docker Compose, GitHub Actions |

## Architecture

The application follows a modular monolith structure:

```text
src/main/java/com/example/marketplace
├── common/
├── security/
├── user/
├── vendor/
├── catalog/
├── inventory/
├── cart/
├── order/
├── payment/
├── commission/
├── notification/
└── report/
```

Core request flow:

```text
Client
  ↓
REST Controller
  ↓
Service
  ↓
Repository
  ↓
PostgreSQL / Redis
```

Modules communicate through interfaces and domain events rather than directly accessing another module's repositories.

## Key Engineering Problems

### 1. Prevent overselling

Inventory will use optimistic locking and atomic updates, with reservation during checkout. Concurrency tests will verify that simultaneous orders cannot consume inventory incorrectly.

### 2. Multi-vendor checkout

One checkout creates one parent `Order` and multiple `SubOrder` records, one for each vendor, inside the required transaction boundary.

### 3. Payment idempotency

Payment callbacks/webhooks may be delivered multiple times. Unique transaction references and idempotency handling prevent duplicate processing.

### 4. Performance

The project will address N+1 queries, database indexes and Redis cache-aside patterns, with measurements documented in the README.

## Development Workflow

```text
main       → stable / release-ready
   ↑
 develop   → integration branch
   ↑
 feature/* → individual tasks
```

### Branch naming

- `feature/<short-description>`
- `fix/<short-description>`
- `refactor/<short-description>`
- `test/<short-description>`
- `docs/<short-description>`
- `chore/<short-description>`

### Commit convention

Use Conventional Commits:

```text
feat: add vendor registration
fix: prevent duplicate payment webhook
refactor: extract inventory service
 test: add checkout concurrency tests
docs: update architecture documentation
chore: configure github actions
```

## Local Development

Coming soon:

```bash
git clone https://github.com/nguyentrung01ute-ui/Multi-vendor-E-commerce-Platform.git
cd Multi-vendor-E-commerce-Platform

docker compose up -d
```

## Documentation

- [Project Specification](docs/project-specification.md)
- [Architecture](docs/architecture.md)
- [Development Guide](docs/development.md)

## Roadmap

1. Project skeleton, Docker, Flyway and CI
2. Authentication, JWT and RBAC
3. Vendor and catalog
4. Cart and inventory
5. Multi-vendor order and checkout
6. Payment and webhook idempotency
7. Commission and reports
8. Redis, async notification, testing and deployment

## Project Highlights

The project is designed to demonstrate production-oriented backend engineering rather than only CRUD APIs: concurrency control, transaction boundaries, idempotency, security ownership checks, query optimization, automated testing and deployment.

## License

This project is for educational and portfolio purposes.
