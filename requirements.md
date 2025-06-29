
# 📘 Backend Feature Requirement Specifications – Airbnb Clone

## 🎯 Objective

This document defines the **technical** and **functional** requirements for key backend features of the Airbnb Clone project. It includes API endpoints, input/output examples, validation rules, and performance criteria.

---

## 1. 🔐 User Authentication

### 📌 Description
Provides secure login, registration, and role-based access for Guests, Hosts, and Admins using JWT.

### 🧩 API Endpoints

| Method | Endpoint               | Description              | Access      |
|--------|------------------------|--------------------------|-------------|
| POST   | `/api/v1/auth/register`| Register as Guest/Host   | Public      |
| POST   | `/api/v1/auth/login`   | Login with credentials   | Public      |
| GET    | `/api/v1/users/me`     | Get current user profile | Authenticated |
| PATCH  | `/api/v1/users/update` | Update profile details   | Authenticated |

### 📝 Input / Output Example

**Register**

```json
Request:
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "SecurePass123",
  "role": "host"
}

Response:
{
  "token": "JWT_TOKEN",
  "user": {
    "id": "user123",
    "email": "jane@example.com",
    "role": "host"
  }
}
````

### ✅ Validation Rules

* Email must be in valid format and unique.
* Password must be at least 8 characters with 1 uppercase and 1 number.
* Role must be one of: `guest`, `host`.

### ⚙️ Performance Criteria

* Authentication must complete in < 500ms.
* Use rate limiting (e.g., 5 login attempts per IP/10min).


## 2. 🏠 Property Management

### 📌 Description

Allows hosts to manage property listings: create, update, view, and delete them. Properties are public for browsing by guests.

### 🧩 API Endpoints

| Method | Endpoint                 | Description                 | Access       |
| ------ | ------------------------ | --------------------------- | ------------ |
| POST   | `/api/v1/properties`     | Create new property listing | Host only    |
| GET    | `/api/v1/properties`     | View all property listings  | Public       |
| GET    | `/api/v1/properties/:id` | View a single property      | Public       |
| PATCH  | `/api/v1/properties/:id` | Update property listing     | Host (owner) |
| DELETE | `/api/v1/properties/:id` | Delete property listing     | Host (owner) |

### 📝 Input / Output Example

**Create Listing**

```json
Request:
{
  "title": "Luxury Apartment",
  "description": "Ocean view with free Wi-Fi",
  "location": "Mombasa, Kenya",
  "price_per_night": 120,
  "max_guests": 4,
  "amenities": ["wifi", "air conditioning"],
  "images": ["image1.jpg", "image2.jpg"]
}

Response:
{
  "id": "prop789",
  "title": "Luxury Apartment",
  "host_id": "user456"
}
```

## 🗂️ File Location

```
alx-airbnb-project-documentation/
└── requirements.md
```