# Development Guide

## Initial workflow

```bash
git clone https://github.com/nguyentrung01ute-ui/Multi-vendor-E-commerce-Platform.git
cd Multi-vendor-E-commerce-Platform
git checkout develop
```

Create a task branch:

```bash
git checkout -b feature/<short-description>
```

## Before opening a PR

```bash
git status
git diff
git log --oneline -10
```

Run the project tests and verify that generated files, IDE metadata, secrets and environment files are not committed.

## Recommended implementation order

1. Spring Boot skeleton
2. Database and Flyway
3. Security / JWT / RBAC
4. User and address
5. Vendor
6. Catalog
7. Inventory
8. Cart
9. Order and checkout
10. Payment
11. Commission
12. Notification and reports
13. Redis/cache and performance optimization
14. Integration/concurrency tests
15. Docker and deployment
