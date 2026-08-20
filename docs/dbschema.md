# Food Delivery Platform Database Design
## PostgreSQL + PostGIS + Redis

---

# Database Overview

## Primary Database
- PostgreSQL
- PostGIS Extension

## Cache Layer
- Redis

## Object Storage
- Amazon S3 / MinIO

---

# Entity Relationship Overview

```text
User
├── Addresses
├── Cart
├── Orders
└── Reviews

Restaurant
├── Restaurant Hours
├── Menu Categories
├── Menu Items
├── Orders
└── Reviews

Menu Category
└── Menu Items

Menu Item
├── Menu Addons
├── Cart Items
└── Order Items

Order
├── Order Items
├── Payment
└── Delivery Partner

Delivery Partner
├── Orders
└── Delivery Locations

Coupon
└── Orders
```

---

# Users

## users

| Column | Type | Constraints |
|----------|----------|----------|
| id | UUID | PK |
| name | VARCHAR(255) | NOT NULL |
| email | VARCHAR(255) | UNIQUE |
| phone | VARCHAR(20) | UNIQUE |
| password_hash | TEXT | NOT NULL |
| profile_image | TEXT | NULL |
| is_active | BOOLEAN | DEFAULT TRUE |
| created_at | TIMESTAMP | |
| updated_at | TIMESTAMP | |

---

# User Addresses

## addresses

| Column | Type | Constraints |
|----------|----------|----------|
| id | UUID | PK |
| user_id | UUID | FK → users.id |
| label | VARCHAR(50) | Home/Work/Other |
| address_line | TEXT | |
| landmark | TEXT | |
| latitude | DECIMAL | |
| longitude | DECIMAL | |
| delivery_instructions | TEXT | |
| is_default | BOOLEAN | |
| created_at | TIMESTAMP | |

Relationship:
- One User → Many Addresses

---

# Restaurants

## restaurants

| Column | Type |
|----------|----------|
| id | UUID |
| owner_id | UUID |
| name | VARCHAR(255) |
| description | TEXT |
| logo_url | TEXT |
| cover_image_url | TEXT |
| phone | VARCHAR(20) |
| email | VARCHAR(255) |
| cuisine | VARCHAR(255) |
| rating | DECIMAL(2,1) |
| total_reviews | INTEGER |
| location | GEOGRAPHY(POINT) |
| address | TEXT |
| status | VARCHAR(50) |
| created_at | TIMESTAMP |

---

# Restaurant Operating Hours

## restaurant_hours

| Column | Type |
|----------|----------|
| id | UUID |
| restaurant_id | UUID |
| day_of_week | INTEGER |
| open_time | TIME |
| close_time | TIME |

Relationship:
- One Restaurant → Many Operating Hours

---

# Menu Categories

## menu_categories

| Column | Type |
|----------|----------|
| id | UUID |
| restaurant_id | UUID |
| name | VARCHAR(255) |
| display_order | INTEGER |

Examples:
- Starters
- Main Course
- Beverages
- Desserts

---

# Menu Items

## menu_items

| Column | Type |
|----------|----------|
| id | UUID |
| restaurant_id | UUID |
| category_id | UUID |
| name | VARCHAR(255) |
| description | TEXT |
| image_url | TEXT |
| price | DECIMAL(10,2) |
| is_veg | BOOLEAN |
| is_available | BOOLEAN |
| is_bestseller | BOOLEAN |
| calories | INTEGER |
| created_at | TIMESTAMP |

Relationship:
- One Category → Many Menu Items

---

# Menu Addons

## menu_addons

| Column | Type |
|----------|----------|
| id | UUID |
| menu_item_id | UUID |
| name | VARCHAR(255) |
| price | DECIMAL(10,2) |

Examples:
- Extra Cheese
- Extra Chicken
- Coke
- Garlic Bread

---

# Shopping Cart

## carts

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| restaurant_id | UUID |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

Relationship:
- One User → One Active Cart

---

# Cart Items

## cart_items

| Column | Type |
|----------|----------|
| id | UUID |
| cart_id | UUID |
| menu_item_id | UUID |
| quantity | INTEGER |
| unit_price | DECIMAL(10,2) |
| created_at | TIMESTAMP |

Relationship:
- One Cart → Many Cart Items

---

# Coupons

## coupons

| Column | Type |
|----------|----------|
| id | UUID |
| code | VARCHAR(100) |
| discount_type | VARCHAR(50) |
| discount_value | DECIMAL(10,2) |
| minimum_order_value | DECIMAL(10,2) |
| maximum_discount | DECIMAL(10,2) |
| expiry_date | TIMESTAMP |
| usage_limit | INTEGER |
| is_active | BOOLEAN |

---

# Orders

## orders

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| restaurant_id | UUID |
| address_id | UUID |
| delivery_partner_id | UUID |
| coupon_id | UUID |
| subtotal | DECIMAL(10,2) |
| delivery_fee | DECIMAL(10,2) |
| platform_fee | DECIMAL(10,2) |
| tax_amount | DECIMAL(10,2) |
| discount_amount | DECIMAL(10,2) |
| total_amount | DECIMAL(10,2) |
| payment_status | VARCHAR(50) |
| order_status | VARCHAR(50) |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

Order Status Values:

```text
PLACED
CONFIRMED
PREPARING
READY
ASSIGNED
PICKED_UP
OUT_FOR_DELIVERY
DELIVERED
CANCELLED
FAILED
REFUNDED
```

---

# Order Items

## order_items

| Column | Type |
|----------|----------|
| id | UUID |
| order_id | UUID |
| menu_item_id | UUID |
| item_name_snapshot | VARCHAR(255) |
| quantity | INTEGER |
| price_snapshot | DECIMAL(10,2) |
| total_price | DECIMAL(10,2) |

Important:
- Snapshot fields preserve historical order accuracy.

---

# Payments

## payments

| Column | Type |
|----------|----------|
| id | UUID |
| order_id | UUID |
| transaction_id | VARCHAR(255) |
| payment_provider | VARCHAR(100) |
| amount | DECIMAL(10,2) |
| status | VARCHAR(50) |
| payment_method | VARCHAR(50) |
| created_at | TIMESTAMP |

Payment Status:

```text
PENDING
SUCCESS
FAILED
REFUNDED
PARTIALLY_REFUNDED
```

---

# Delivery Partners

## delivery_partners

| Column | Type |
|----------|----------|
| id | UUID |
| name | VARCHAR(255) |
| phone | VARCHAR(20) |
| vehicle_type | VARCHAR(50) |
| vehicle_number | VARCHAR(50) |
| rating | DECIMAL(2,1) |
| total_deliveries | INTEGER |
| is_online | BOOLEAN |
| created_at | TIMESTAMP |

---

# Delivery Tracking

## delivery_locations

| Column | Type |
|----------|----------|
| id | UUID |
| delivery_partner_id | UUID |
| latitude | DECIMAL |
| longitude | DECIMAL |
| recorded_at | TIMESTAMP |

Purpose:
- Live order tracking
- Route visualization
- ETA calculations

---

# Reviews

## reviews

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| restaurant_id | UUID |
| order_id | UUID |
| rating | INTEGER |
| comment | TEXT |
| created_at | TIMESTAMP |

Relationship:
- One User → Many Reviews
- One Restaurant → Many Reviews

---

# Favorites

## favorite_restaurants

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| restaurant_id | UUID |
| created_at | TIMESTAMP |

Purpose:
- Saved restaurants
- Personalized recommendations

---

# Notifications

## notifications

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| title | VARCHAR(255) |
| message | TEXT |
| type | VARCHAR(50) |
| is_read | BOOLEAN |
| created_at | TIMESTAMP |

Examples:
- Order updates
- Payment updates
- Promotional notifications

---

# Recommended PostgreSQL Indexes

```sql
CREATE INDEX idx_orders_user_id
ON orders(user_id);

CREATE INDEX idx_orders_restaurant_id
ON orders(restaurant_id);

CREATE INDEX idx_orders_status
ON orders(order_status);

CREATE INDEX idx_menu_items_restaurant_id
ON menu_items(restaurant_id);

CREATE INDEX idx_reviews_restaurant_id
ON reviews(restaurant_id);

CREATE INDEX idx_restaurants_location
ON restaurants
USING GIST(location);
```

---

# Redis Usage

## OTP Storage

```text
otp:{phone_number}
```

---

## Active User Sessions

```text
session:{user_id}
```

---

## Popular Searches

```text
popular_searches
```

---

## Restaurant Cache

```text
restaurant:{restaurant_id}
```

---

## Active Delivery Tracking

```text
delivery:{partner_id}
```

---

# Database Design Principles

1. PostgreSQL is the source of truth.
2. Redis is used only for caching and temporary data.
3. Historical order information is preserved through snapshot fields.
4. PostGIS handles all geospatial queries.
5. UUIDs are used as primary keys.
6. All critical entities contain audit timestamps.
7. Soft-delete can be added later for compliance and recovery.
8. Schema is optimized for an MVP-to-production scale food delivery platform.