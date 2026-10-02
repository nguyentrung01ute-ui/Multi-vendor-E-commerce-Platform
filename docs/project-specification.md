# Project Specification

## 1. Overview

Multi-vendor E-commerce Platform là backend marketplace nhiều nhà bán hàng kiểu Shopee/Lazada, xây dựng bằng Java Spring Boot theo kiến trúc modular monolith.

### Goals

- Cho phép nhiều vendor cùng bán hàng trên một nền tảng.
- Xử lý phân quyền, tách đơn theo vendor, tồn kho đồng thời, thanh toán và hoa hồng.
- Đảm bảo test, Docker, CI/CD, API documentation và khả năng demo.

## 2. Actors

| Role | Responsibility |
|---|---|
| GUEST | Browse products, search, register/login |
| CUSTOMER | Cart, address, checkout, orders |
| VENDOR | Store, products, inventory, own orders, revenue |
| ADMIN | Vendor approval, categories, commission and reports |

## 3. Main Modules

- `common`
- `security`
- `user`
- `vendor`
- `catalog`
- `inventory`
- `cart`
- `order`
- `payment`
- `commission`
- `notification`
- `report`

## 4. Database Core

Main tables:

`users`, `roles`, `user_roles`, `addresses`, `vendors`, `categories`, `products`, `product_variants`, `product_images`, `inventory`, `carts`, `cart_items`, `orders`, `sub_orders`, `order_items`, `payments`, `refunds`, `commission_ledger`, `payouts`, `idempotency_keys` and optionally `outbox_events`.

Important design rules:

- Snapshot product price/name and delivery address into order data.
- Use `DECIMAL(19,2)` / `BigDecimal` for money.
- Index vendor/category/status product queries, user order history, vendor sub-orders and unique payment transaction references.
- Use soft delete for product/vendor where appropriate.

## 5. Order Flow

A checkout creates one parent order and N sub-orders, grouped by vendor.

```text
PENDING_PAYMENT
      ↓
    PAID
      ↓
 CONFIRMED
      ↓
  PACKING
      ↓
  SHIPPING
      ↓
 DELIVERED
      ↓
 COMPLETED
```

Cancellation/refund transitions must be explicitly controlled.

## 6. Technical Requirements

### Inventory

Use optimistic locking plus atomic update/reservation. Concurrency tests should verify that inventory cannot be oversold.

### Transactions

Checkout groups cart items by vendor and creates the order, sub-orders, order items and inventory reservations within the required transaction boundary.

### Idempotency

Checkout supports `Idempotency-Key`. Payment processing uses a unique transaction reference so repeated callbacks remain safe.

### Security

Use BCrypt, input validation, parameterized persistence, CORS configuration, rate limiting and ownership checks to prevent cross-vendor access.

### Performance

Investigate N+1 queries, use appropriate indexes, Redis cache-aside and measure improvements with load testing where applicable.

## 7. API Version

All REST APIs use:

```text
/api/v1/...
```

Responses should follow a consistent structure:

```json
{
  "success": true,
  "data": {},
  "error": null,
  "timestamp": "..."
}
```

## 8. Testing

- Unit tests with JUnit 5 and Mockito.
- Integration tests with Testcontainers.
- API tests with RestAssured.
- Concurrency tests for inventory.
- Duplicate webhook tests for idempotency.
- Optional load testing with k6/JMeter.

## 9. Roadmap

### Phase 0

Requirements, ERD, project setup, Docker, Flyway and CI.

### Phase 1

JWT authentication, RBAC, users, addresses and global exception handling.

### Phase 2

Vendor approval, category, product, variants, images, search and pagination.

### Phase 3

Cart, inventory, reservation and concurrency protection.

### Phase 4

Checkout, sub-orders, state machine, payment, webhook and auto-cancel.

### Phase 5

Commission ledger, payout and dashboards.

### Phase 6

Redis cache, async email, testing, optimization, README and deployment.
