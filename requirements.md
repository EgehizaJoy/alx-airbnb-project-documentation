# Airbnb Clone - Technical and Functional Requirements

This document outlines the technical and functional requirements for three core backend features of the Airbnb Clone project.

---

## 1. User Authentication

### 🔧 API Endpoints:
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/users/me`

### 📥 Input Specifications:
- `email`: valid, unique, required
- `password`: min 8 characters, required
- `role`: enum (guest, host), default: guest
- `first_name`, `last_name`: required

### 📤 Output:
- On success: JWT token, user profile
- On failure: HTTP 400/401 with error message

### ✅ Validation Rules:
- Email must be in correct format and unique
- Password must be hashed
- Role must be validated

### ⚙️ Performance Criteria:
- Response time < 300ms
- Token must be valid for 24 hours
- Password hashing: bcrypt with 12+ salt rounds

---

## 2. Property Management

### 🔧 API Endpoints:
- `POST /api/properties/`
- `GET /api/properties/`
- `PUT /api/properties/:id`
- `DELETE /api/properties/:id`

### 📥 Input Specifications:
- `title`, `description`, `location`, `price_per_night`: required
- `amenities`, `images`: optional
- `host_id`: extracted from JWT

### 📤 Output:
- On success: property object with host ID
- On failure: HTTP 400/403/404

### ✅ Validation Rules:
- Host must be authenticated
- Price must be a positive decimal
- Description must not exceed 2000 characters

### ⚙️ Performance Criteria:
- Indexed by location and price
- Response < 500ms for filtered listing requests
- Max 100 results per page with pagination

---

## 3. Booking System

### 🔧 API Endpoints:
- `POST /api/bookings/`
- `GET /api/bookings/:id`
- `GET /api/users/:id/bookings`
- `DELETE /api/bookings/:id`

### 📥 Input Specifications:
- `property_id`: required
- `start_date`, `end_date`: required and valid range
- `user_id`: extracted from JWT

### 📤 Output:
- On success: booking confirmation with status
- On failure: conflict message (e.g., double booking)

### ✅ Validation Rules:
- Dates must not overlap existing bookings
- User cannot book their own listing
- Total price = (days * property price)

### ⚙️ Performance Criteria:
- Prevent double booking using date constraints
- Indexed by user ID and property ID
- Response time < 400ms
