
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

### ✅ Validation Rules

* Title max length: 100 chars, required.
* Price must be a positive number.
* Must include at least one image and one amenity.
* Hosts can only manage their own listings.

### ⚙️ Performance Criteria

* GET requests must respond in < 300ms.
* Pagination must be supported on property list.

---

## 3. 📆 Booking System

### 📌 Description

Enables guests to book properties for specific dates. Prevents double bookings and supports cancellations.

### 🧩 API Endpoints

| Method | Endpoint               | Description      | Access        |
| ------ | ---------------------- | ---------------- | ------------- |
| POST   | `/api/v1/bookings`     | Create a booking | Guest only    |
| GET    | `/api/v1/bookings/me`  | View my bookings | Authenticated |
| PATCH  | `/api/v1/bookings/:id` | Cancel a booking | Guest/Host    |

### 📝 Input / Output Example

**Create Booking**

```json
Request:
{
  "property_id": "prop789",
  "check_in": "2025-07-01",
  "check_out": "2025-07-05"
}

Response:
{
  "booking_id": "book123",
  "status": "confirmed",
  "total_price": 480
}
```

### ✅ Validation Rules

* Dates must be valid and check-in < check-out.
* Property must not be already booked for that range.
* Guests cannot book their own properties.

### ⚙️ Performance Criteria

* Booking conflict check must be done in < 200ms.
* Use concurrency control to prevent race conditions.

---

## ✅ Common Rules Across All Features

* **Authentication**: All protected routes require `Authorization: Bearer <JWT>`.
* **Error Responses** follow this pattern:

```json
{
  "error": "Validation failed",
  "details": {
    "email": "Email is already in use"
  }
}
```

* **Status Codes**:

  * 200 OK / 201 Created
  * 400 Bad Request
  * 401 Unauthorized
  * 403 Forbidden
  * 404 Not Found
  * 500 Server Error

---

## 🧠 Suggestions for Future Features

* Add real-time notifications via WebSocket.
* Include refund system and cancellation policy logic.
* Add support for multi-language and currency localization.

---

## 🗂️ File Location

```
alx-airbnb-project-documentation/
└── requirements.md
```