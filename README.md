# Online Bookstore System

A full-stack e-commerce web application for browsing and purchasing books with secure authentication, payment processing, and administrative management.

**Live Demo:** https://obs-bookstore.duckdns.org

## Overview

This project implements a complete online bookstore with user authentication, shopping cart, Stripe payment integration, and administrative controls. The application follows MVC architecture with 16 controllers, 10 services, and 7 repositories, and is deployed on AWS with SSL/TLS encryption.

## Technology Stack

**Backend:**
- Java 17 with Spring Boot 3.5.3
- Spring Security 6 - Role-based access control (RBAC), BCrypt password encryption, CSRF protection, Remember Me
- Spring Data JPA with Hibernate ORM
- MySQL 8.0 - Relational database with 7 entities
- Spring Mail - Gmail SMTP integration for transactional emails

**Frontend:**
- Thymeleaf server-side templates (19 pages)
- Bootstrap 4 - Responsive design
- JavaScript - Dynamic cart updates and form validation

**Infrastructure & DevOps:**
- Docker & Docker Compose - Multi-container orchestration
- AWS EC2 - Application hosting
- AWS RDS - Managed MySQL database
- AWS ECR - Docker image registry
- Nginx - Reverse proxy with load balancing capability
- Let's Encrypt - Free SSL/TLS certificates for HTTPS

**External APIs:**
- Stripe API - Payment processing and tokenization

**Build Tools:**
- Maven - Dependency management and build automation

## Architecture

The application follows a layered MVC architecture:

```
├── Presentation Layer
│   ├── 16 Controllers          # HTTP request handling, routing, view rendering
│   └── 19 Thymeleaf templates  # Server-side rendered HTML
│
├── Business Logic Layer
│   └── 10 Services             # Core business logic (user auth, cart, orders, email, payments)
│
├── Data Access Layer
│   ├── 7 Repositories          # Spring Data JPA interfaces
│   └── 7 Entity Models         # JPA entities mapped to database tables
│
└── Cross-Cutting Concerns
    ├── Security Config         # Spring Security filter chain, authentication
    └── Password Encoder        # BCrypt configuration
```

**Design Patterns:**
- MVC (Model-View-Controller)
- Repository pattern for data access
- Service layer for business logic encapsulation
- Dependency injection throughout

## Core Features

### User Management
- **Registration** - Account creation with email verification workflow
- **Authentication** - Secure login with BCrypt-hashed passwords
- **Authorization** - Role-based access (USER, ADMIN roles)
- **Remember Me** - Persistent login sessions with secure tokens
- **Password Reset** - Email-based password recovery with time-limited tokens
- **Email Verification** - Token-based account activation

### E-Commerce Functionality
- **Product Catalog** - Browse books with search by title, author, genre
- **Shopping Cart** - Session-based cart with persistent storage for authenticated users
- **Checkout Process** - Multi-step checkout with shipping address capture
- **Promotional Codes** - Discount system with percentage/fixed amount off
- **Payment Processing** - Stripe integration with secure card tokenization
- **Order Management** - Order history, confirmation emails, tracking

### Admin Dashboard
- **Book Management** - Full CRUD operations on product catalog with image upload
- **User Administration** - View, manage, and delete user accounts
- **Order Viewing** - Access all customer orders and details
- **Promo Code Management** - Create, update, and deactivate promotional codes

### Security Features
- **Spring Security** - Filter chain with authentication and authorization
- **Password Encryption** - BCrypt hashing with salt
- **CSRF Protection** - Token-based protection on all state-changing operations
- **SSL/TLS** - HTTPS encryption for all traffic
- **Card Data Encryption** - Payment information stored securely
- **Session Management** - Secure session handling with timeout

### Email System
Automated transactional emails using Gmail SMTP:
- Account verification emails with activation links
- Welcome emails post-verification
- Password reset with secure tokens (1-hour expiry)
- Order confirmation with order details

## Database Schema

**7 Relational Tables** managed via JPA/Hibernate:

- `users` - User accounts (credentials, profile, verification status)
- `books` - Product catalog (title, author, genre, price, stock, images)
- `orders` - Purchase records (order number, totals, shipping address, timestamps)
- `order_items` - Line items for each order (book reference, quantity, price snapshot)
- `cart_items` - Shopping cart persistence (user session, quantities)
- `cards` - Encrypted payment methods (tokenized card data)
- `promo_codes` - Discount codes (code, type, amount, usage limits)

**Key Relationships:**
- User → Orders (One-to-Many)
- User → Cart Items (One-to-Many)
- Order → Order Items (One-to-Many)
- Book → Cart Items (One-to-Many)
- Book → Order Items (One-to-Many)

## Deployment

**Production Environment:**
- **Hosting:** AWS EC2 t3.micro instance
- **Database:** AWS RDS MySQL 8.0 (db.t3.micro)
- **Container Registry:** AWS ECR for Docker images
- **Web Server:** Nginx reverse proxy (handles SSL termination, forwards to app on port 8080)
- **SSL Certificate:** Let's Encrypt with automatic renewal
- **Domain:** DuckDNS for dynamic DNS
- **Container Orchestration:** Docker with health checks and auto-restart policies

## Quick Start with Docker

1. Ensure Docker Desktop is running
2. Clone the repository
3. Navigate to the project directory: `cd obs`
4. Run the application:

```bash
docker-compose up -d --build
```

The application will be available at: http://localhost:8080

**Admin Login:**
- Username: `admin`
- Password: `password`

**Database Access:**
- Host: `localhost`
- Port: `3307`
- Database: `obs_db`
- Username: `obs_user`
- Password: `obs_password`

## Manual Setup

### Prerequisites
- Java 17
- Maven 3.6+
- MySQL 8.0

### Database Setup
1. Create MySQL database:
```sql
CREATE DATABASE obs_db;
```

2. Copy `application-template.properties` to `application.properties`
3. Update database credentials in `application.properties`:
```properties
spring.datasource.username=your_username
spring.datasource.password=your_password
```

### Running the Application
```bash
./mvnw spring-boot:run
```

## Project Structure

```
src/main/java/com/example/obs/
├── config/          # Security and password configuration
├── controller/      # Web controllers (Auth, Cart, Checkout, Profile, etc.)
├── model/          # Domain entities (User, Book, Order, PromoCode)
├── repository/     # Data access layer
├── service/        # Business logic (Email, PromoCode, User services)
└── ObsApplication.java
```

## Available URLs

### Public Routes
- `/` - Homepage
- `/book/{id}` - Book details
- `/search` - Search books by title/author/genre
- `/login` - Login page (with Remember Me)
- `/register` - User registration (with promotions signup)
- `/forgot-password` - Password reset request
- `/reset-password` - Password reset form
- `/verify-email` - Email verification

### User Routes (Authenticated)
- `/cart` - Shopping cart
- `/checkout` - Checkout process (with promo codes)
- `/profile` - User profile (with promotions settings)

### Admin Routes
- `/admin/books` - Manage books
- `/admin/users` - Manage users
- `/admin/orders` - View orders
- `/admin/promo-codes` - Manage promo codes

## Features

- **User Authentication**: Registration, login, logout, email verification
- **Password Management**: Forgot/reset password via email
- **Shopping**: Browse books, search, cart, checkout with promo codes
- **Promotions**: User signup for promotional emails, admin-managed promo codes
- **Email Integration**: Verification, password reset, welcome emails
- **Admin Panel**: Manage books, users, orders, and promo codes
- **Security**: Remember me, CSRF protection, encrypted passwords
- **Responsive Design**: Bootstrap 4 with custom styling
