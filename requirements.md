
# 🛠️ Backend Requirement Specifications

This document outlines detailed requirement specifications for three key backend features of the platform: **User Authentication**, **Property Management**, and the **Booking System**.

---

## 🔐 1. User Authentication

### ✅ Functional Requirements
- Allow users to register and log in securely.
- Generate and validate JWT tokens.
- Encrypt passwords before storing in the database.
- Provide protected user profile access.

### 🌐 API Endpoints

| Method | Endpoint           | Description               |
|--------|--------------------|---------------------------|
| POST   | `/api/auth/register` | Register a new user      |
| POST   | `/api/auth/login`    | Authenticate a user      |
| GET    | `/api/auth/me`       | Get current user profile (protected route) |

### 📥 Input / 📤 Output

#### `POST /api/auth/register`
**Input:**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "SecurePassword123"
}
```

**Validation Rules:**
- Email must be valid and unique.
- Password must be ≥ 8 characters, alphanumeric.
- Name must not be empty.

**Output:**
```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "u123",
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

---

#### `POST /api/auth/login`
**Input:**
```json
{
  "email": "jane@example.com",
  "password": "SecurePassword123"
}
```

**Output:**
```json
{
  "token": "jwt_token_here",
  "user": {
    "id": "u123",
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

### ⚙️ Performance Criteria
- Token generation: < 300ms
- Concurrent login support: 1000 users
- Rate limit: 5 failed login attempts/minute/IP

---

## 🏠 2. Property Management

### ✅ Functional Requirements
- Hosts can add, update, and delete property listings.
- Listings include location, images, amenities, and availability.
- Support property filtering and searching.

### 🌐 API Endpoints

| Method | Endpoint             | Description                   |
|--------|----------------------|-------------------------------|
| POST   | `/api/properties`    | Create a new listing          |
| GET    | `/api/properties`    | Get all listings              |
| GET    | `/api/properties/:id`| Get a specific listing        |
| PUT    | `/api/properties/:id`| Update an existing listing    |
| DELETE | `/api/properties/:id`| Delete a listing              |

### 📥 Input / 📤 Output

#### `POST /api/properties`
**Input:**
```json
{
  "title": "2-Bedroom Apartment in Lagos",
  "description": "Spacious apartment near Lekki.",
  "location": "Lagos",
  "pricePerNight": 120,
  "amenities": ["WiFi", "AC", "Parking"],
  "images": ["url1", "url2"],
  "availability": {
    "from": "2025-06-01",
    "to": "2025-06-30"
  }
}
```

**Validation Rules:**
- Title, description, and location must be non-empty strings.
- `pricePerNight` must be a positive number.
- `images` must contain valid URLs.
- Availability dates must be valid and chronological.

**Output:**
```json
{
  "id": "prop123",
  "message": "Property listed successfully."
}
```

### ⚙️ Performance Criteria
- Listing creation: < 400ms
- Search response with filters: < 800ms
- Concurrent listing updates: 500 users

---

## 📅 3. Booking System

### ✅ Functional Requirements
- Guests can request bookings.
- Hosts can approve or reject requests.
- System must avoid overlapping bookings.

### 🌐 API Endpoints

| Method | Endpoint              | Description                     |
|--------|-----------------------|---------------------------------|
| POST   | `/api/bookings`       | Create a new booking request    |
| GET    | `/api/bookings`       | Get user's or host's bookings   |
| PUT    | `/api/bookings/:id`   | Accept or reject a booking      |
| DELETE | `/api/bookings/:id`   | Cancel a booking                |

### 📥 Input / 📤 Output

#### `POST /api/bookings`
**Input:**
```json
{
  "propertyId": "prop123",
  "checkIn": "2025-06-05",
  "checkOut": "2025-06-10",
  "guests": 2
}
```

**Validation Rules:**
- Check-in and check-out must fall within availability.
- No overlapping booking must exist for the same property.
- `guests` must be a positive integer.

**Output:**
```json
{
  "id": "bk101",
  "status": "pending",
  "message": "Booking request submitted."
}
```

### ⚙️ Performance Criteria
- Booking creation: < 500ms
- Conflict checking: < 300ms
- Concurrent bookings: 2000 requests

---

> **Note:** All protected endpoints must use `Authorization: Bearer <JWT>` in headers.
