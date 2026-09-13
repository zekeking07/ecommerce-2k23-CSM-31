# Sprint 1: Architecture & Scope Definition

**Project:** ArtisanHub — A Marketplace for Local Artisan & Handmade Goods  
**Sprint:** 1 of 6  
**Author:** Bushra Batool — 2k23/CSM/31

---

## 1. Target Audience & Market Focus

**Market Vertical:** Niche E-Commerce — Handmade, Small-Batch, and Local Artisan Goods

ArtisanHub focuses on handmade and locally produced products such as home décor, jewelry, pottery, candles, and textiles.

### User Persona

| Attribute | Detail |
|---|---|
| Name | Sara, 27 |
| Occupation | Marketing Executive, Urban Professional |
| Goal | Wants to buy unique, locally-made gifts and home items instead of mass-produced products |
| Pain Points | Struggles to discover trustworthy local sellers online and wants to know who made the product |
| Behavior | Shops mostly on mobile, checks seller information and reviews, and prefers trusted sellers |

### Core Problem

Local artisans often lack an affordable and easy-to-use online storefront, while buyers looking for authentic handmade products do not have a single trustworthy platform to discover them.

ArtisanHub connects local artisans with buyers through product discovery, category-based browsing, seller product listings, cart management, and a simple checkout and order process.

### Secondary Persona

Small-scale sellers and artisans who need a low-friction platform to list products, manage inventory, and receive customer orders without building their own e-commerce infrastructure.

---

## 2. MVP Feature Scope

The MVP contains six primary user workflows that are feasible within the academic semester.

| Category | Feature | Description | Priority |
|---|---|---|---|
| User Management | Registration & Login | Buyers and sellers can register, log in, and manage a basic profile using JWT-based authentication. | High (MVP) |
| Catalog | Product Listing & Categories | Sellers can create and edit products under predefined categories, while buyers can browse products by category. | High (MVP) |
| Discovery | Search & Filter | Buyers can search products by name or keyword and filter products by category and price range. | Medium |
| Shopping | Cart Management | Buyers can add, remove, and update product quantities in a persistent account-based cart. | High (MVP) |
| Orders | Checkout & Order Placement | Buyers can convert their cart into an order by providing shipping details and placing the order. | High (MVP) |
| Orders | Order History | Buyers can view previous orders, while sellers can view orders related to their products. | Medium |

Payment gateway integration, product reviews, ratings, and advanced recommendation systems are intentionally deferred to later sprints to keep the MVP feasible within the project timeline.

---

## 3. Tech Stack Selection & Justification

| Layer | Choice | Justification |
|---|---|---|
| Frontend | **React (Vite)** | React provides a component-based architecture suitable for reusable e-commerce interfaces such as product cards, product pages, cart items, and checkout components. Vite provides a fast development environment and keeps the frontend setup lightweight. |
| Backend | **Node.js + Express** | Node.js allows JavaScript to be used across the application stack, reducing context switching for a solo developer. Express is lightweight, well-documented, and suitable for developing REST APIs within the six-sprint academic project. |
| Database | **PostgreSQL** | PostgreSQL is suitable because the application contains strongly related entities such as Users, Products, Orders, Order_Items, Categories, and Cart_Items. Its relational structure, foreign keys, and transaction support provide strong data integrity. |
| Caching (Optional) | **Redis** | Redis can be used for caching frequently accessed data such as product categories and featured products. This can reduce repeated database queries as the application grows. |

---

## 4. Entity-Relationship Diagram (ERD)

The database schema contains the following core entities:

- Users
- Products
- Categories
- Orders
- Order_Items
- Cart
- Cart_Items

### Mermaid ERD

```mermaid
erDiagram

    USERS ||--o{ PRODUCTS : "sells"
    USERS ||--|| CART : "owns"
    USERS ||--o{ ORDERS : "places"

    CATEGORIES ||--o{ PRODUCTS : "classifies"

    CART ||--o{ CART_ITEMS : "contains"
    PRODUCTS ||--o{ CART_ITEMS : "added as"

    ORDERS ||--o{ ORDER_ITEMS : "contains"
    PRODUCTS ||--o{ ORDER_ITEMS : "ordered as"


    USERS {
        INTEGER user_id PK
        VARCHAR name
        VARCHAR email
        VARCHAR password_hash
        VARCHAR role
        TIMESTAMP created_at
    }

    CATEGORIES {
        INTEGER category_id PK
        VARCHAR name
        TEXT description
    }

    PRODUCTS {
        INTEGER product_id PK
        INTEGER seller_id FK
        INTEGER category_id FK
        VARCHAR title
        TEXT description
        DECIMAL price
        INTEGER stock_quantity
        TIMESTAMP created_at
    }

    CART {
        INTEGER cart_id PK
        INTEGER user_id FK
        TIMESTAMP updated_at
    }

    CART_ITEMS {
        INTEGER cart_item_id PK
        INTEGER cart_id FK
        INTEGER product_id FK
        INTEGER quantity
    }

    ORDERS {
        INTEGER order_id PK
        INTEGER user_id FK
        DECIMAL total_amount
        VARCHAR status
        VARCHAR shipping_address
        TIMESTAMP order_date
    }

    ORDER_ITEMS {
        INTEGER order_item_id PK
        INTEGER order_id FK
        INTEGER product_id FK
        INTEGER quantity
        DECIMAL price_at_purchase
    }
