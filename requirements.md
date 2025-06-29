# 🧾 Backend Feature Requirements – Airbnb Clone

## 🛠️ Technology Stack

| Technology             | Purpose                                           |
|------------------------|---------------------------------------------------|
| Django                 | Backend logic using Python web framework         |
| Django REST Framework  | RESTful API support                              |
| PostgreSQL             | Relational database                              |
| JWT (SimpleJWT)        | Token-based user authentication                  |
| Celery                 | Asynchronous background tasks (emails, etc.)     |
| Redis                  | Caching and task queue backend for Celery        |
| Docker                 | Containerized development and deployment         |
| GitHub Actions         | Continuous Integration and Deployment (CI/CD)    |

---

## 🔐 Feature 1: User Authentication

### Functionalities

- Register as guest or host
- Login using email and password
- Secure token generation using JWT
- Token refresh and logout
- Password reset support
- Profile update

### API Endpoints

| Method | Endpoint                     | Description                          |
|--------|------------------------------|--------------------------------------|
| POST   | /api/auth/register/          | Register a new user                  |
| POST   | /api/auth/login/             | Login and receive token              |
| POST   | /api/auth/logout/            | Logout and invalidate token          |
| POST   | /api/auth/password-reset/    | Request password reset link          |
| PUT    | /api/users/profile/          | Update user profile                  |

### Input/Output Example

**Input (Registration):**
```json
{
  "email": "jane@example.com",
  "password": "Pass1234!",
  "first_name": "Jane",
  "last_name": "Doe",
  "role": "host"
}
````

**Output:**

```json
{
  "user": {
    "id": "user-001",
    "email": "jane@example.com",
    "token": "jwt_token_here"
  }
}
```

### Validation Rules

* Email must be unique and valid
* Password must contain uppercase, lowercase, number (min. 8 chars)
* Role must be either guest or host

### Performance Criteria

* Authentication response within 500ms
* Rate limiting: max 5 failed login attempts per 10 minutes per IP

---

## 🏠 Feature 2: Property Management

### Functionalities

* Hosts can create, update, and delete their own listings
* Guests can browse property listings
* Admins can moderate listings

### API Endpoints

| Method | Endpoint              | Description                       |
| ------ | --------------------- | --------------------------------- |
| GET    | /api/properties/      | List all available properties     |
| POST   | /api/properties/      | Create a new property (host only) |
| GET    | /api/properties/<id>/ | View a single property            |
| PUT    | /api/properties/<id>/ | Update a property (host only)     |
| DELETE | /api/properties/<id>/ | Delete a property (host only)     |

### Input/Output Example

**Input:**

```json
{
  "name": "Modern Loft in Kampala",
  "description": "Stylish 1-bedroom with kitchen and balcony",
  "location": "Kampala, Uganda",
  "price_per_night": 85,
  "max_guests": 2,
  "amenities": ["wifi", "AC"],
  "images": ["img1.jpg", "img2.jpg"]
}
```

**Output:**

```json
{
  "property_id": "prop-001",
  "name": "Modern Loft in Kampala",
  "price_per_night": 85,
  "created_at": "2025-06-29T17:00:00Z"
}
```

### Validation Rules

* Name required (max 100 chars)
* Price must be a positive number
* Host can only edit their own listings

### Performance Criteria

* Support pagination & filters
* GET /properties/ returns within 300ms

---

## 📆 Feature 3: Booking System

### Functionalities

* Guests can book available properties
* Hosts and guests are notified of bookings
* Admins can cancel bookings if necessary

### API Endpoints

| Method | Endpoint                   | Description                       |
| ------ | -------------------------- | --------------------------------- |
| POST   | /api/bookings/             | Create a new booking              |
| GET    | /api/bookings/             | View user’s own bookings          |
| GET    | /api/bookings/<id>/        | View booking details              |
| PUT    | /api/bookings/<id>/cancel/ | Cancel booking (host/guest/admin) |

### Input/Output Example

**Input:**

```json
{
  "property_id": "prop-001",
  "start_date": "2025-07-15",
  "end_date": "2025-07-18",
  "total_price": 255
}
```

**Output:**

```json
{
  "booking_id": "book-789",
  "status": "confirmed",
  "created_at": "2025-06-29T18:00:00Z"
}
```

### Validation Rules

* Check-in must be before check-out
* No overlapping bookings for same property
* Guests cannot book their own properties

### Performance Criteria

* Conflict checking must happen in under 200ms
* Bookings should trigger confirmation email async via Celery

---

## ✅ Common Requirements

* All protected endpoints require JWT in Authorization header
* Standard response codes: 200, 201, 400, 401, 403, 404, 500
* All API responses are JSON
* Errors return structured details:

```json
{
  "error": "Validation error",
  "message": "Email already exists"
}
```

## 📂 File Location

```
alx-airbnb-project-documentation/
└── requirements.md
```