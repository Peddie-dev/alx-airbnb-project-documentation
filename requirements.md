# Airbnb Clone Backend - Technical & Functional Requirements

## 1. User Authentication

### Description
Handles guest and host registration, login, and profile management with secure authentication.

### API Endpoints

| Endpoint                | Method | Description                        | Input                                   | Output                                |
|-------------------------|--------|------------------------------------|----------------------------------------|---------------------------------------|
| /api/auth/register      | POST   | Register new user                  | { name, email, password, role }        | { userId, name, email, role, token } |
| /api/auth/login         | POST   | Login user                         | { email, password }                     | { userId, name, email, role, token } |
| /api/auth/profile       | GET    | Get user profile                   | Auth token                             | { userId, name, email, role, profile }|
| /api/auth/profile       | PUT    | Update profile                     | Auth token, { name, email, photo }     | { userId, name, email, profile }     |

### Validation Rules
- Email must be valid and unique.  
- Password must be at least 8 characters, include letters and numbers.  
- Role must be either `guest` or `host`.

### Performance Criteria
- Registration/Login must respond within 200ms for 95% of requests.  
- JWT tokens must expire after 24 hours for security.  
- API must handle 500 concurrent login requests without failure.

---

## 2. Property Management

### Description
Allows hosts to create, update, delete, and view property listings.

### API Endpoints

| Endpoint                  | Method | Description                        | Input                                                      | Output                            |
|----------------------------|--------|------------------------------------|-----------------------------------------------------------|-----------------------------------|
| /api/properties            | POST   | Add new property                   | { title, description, location, price, amenities, photos }| { propertyId, status: created }  |
| /api/properties/:id        | PUT    | Update property                    | { title, description, location, price, amenities, photos }| { propertyId, status: updated }  |
| /api/properties/:id        | DELETE | Delete property                    | propertyId                                                | { propertyId, status: deleted }  |
| /api/properties            | GET    | List all properties                | { filters: location, price, amenities, guests }           | [{ propertyId, title, price, amenities }] |
| /api/properties/:id        | GET    | Get property details               | propertyId                                                | { propertyId, title, description, price, amenities, host } |

### Validation Rules
- Title and description cannot be empty.  
- Price must be a positive number.  
- Availability dates must not overlap existing bookings.  
- Photos must be valid image URLs or uploaded to cloud storage.

### Performance Criteria
- Search/filter results must return within 500ms for 95% of requests.  
- API must support pagination for large datasets (>10,000 properties).  

---

## 3. Booking System

### Description
Handles property bookings, cancellations, and booking status tracking.

### API Endpoints

| Endpoint                  | Method | Description                        | Input                                   | Output                                |
|----------------------------|--------|------------------------------------|----------------------------------------|---------------------------------------|
| /api/bookings              | POST   | Create a booking                   | { propertyId, userId, startDate, endDate } | { bookingId, status: pending }        |
| /api/bookings/:id          | PUT    | Update booking                     | { status }                              | { bookingId, status }                 |
| /api/bookings/:id/cancel   | POST   | Cancel booking                     | bookingId                               | { bookingId, status: canceled }      |
| /api/bookings/user/:id     | GET    | Get bookings for a user            | userId                                  | [{ bookingId, property, status, dates }] |
| /api/bookings/property/:id | GET    | Get bookings for a property        | propertyId                              | [{ bookingId, guest, status, dates }] |

### Validation Rules
- Booking dates must not overlap existing bookings.  
- Only `pending` or `confirmed` bookings can be canceled.  
- PropertyId and UserId must exist in their respective databases.  

### Performance Criteria
- Booking creation must complete within 300ms.  
- Must handle at least 500 concurrent booking requests per property.  
- All changes must be transaction-safe to avoid double-booking.

---


