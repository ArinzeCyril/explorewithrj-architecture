# API Structure

This document outlines the API endpoints for the ExploreWithRJ application. All endpoints are prefixed with `/.netlify/functions/`.

## Authentication

Handles user registration, login, and password management.

-   **`POST /auth-register`**
    -   Description: Registers a new user.
    -   Body: `{ "fullName": "String", "email": "String", "password": "String" }`
    -   Response: `{ "message": "String", "user": "Object", "token": "String" }`

-   **`POST /auth-login`**
    -   Description: Authenticates a user and returns a JWT.
    -   Body: `{ "email": "String", "password": "String" }`
    -   Response: `{ "message": "String", "user": "Object", "token": "String" }`

-   **`PUT /auth-update-password`**
    -   Description: Updates the password for the authenticated user.
    -   Auth: Required.
    -   Body: `{ "oldPassword": "String", "newPassword": "String" }`
    -   Response: `{ "message": "String" }`

-   **`GET /auth-verify`**
    -   Description: Verifies the authenticity of a JWT.
    -   Auth: Required.
    -   Response: `{ "user": "Object" }`

---

## Admin

Admin-only endpoints for managing users and viewing site data.

-   **`GET /admin-dashboard`**
    -   Description: Retrieves aggregated data for the admin dashboard.
    -   Auth: Admin Required.
    -   Response: `{ "data": "Object" }`

-   **`GET /admin-users`**
    -   Description: Retrieves a list of all users.
    -   Auth: Admin Required.
    -   Response: `[{ "user_data" }]`

-   **`GET /admin-users/{id}`**
    -   Description: Retrieves details for a specific user.
    -   Auth: Admin Required.
    -   Response: `{ "user_data" }`

-   **`PUT /admin-users/{id}`**
    -   Description: Updates the role of a specific user.
    -   Auth: Admin Required.
    -   Body: `{ "role": "String" }`
    -   Response: `{ "message": "String" }`

---

## Appointments

Endpoints for managing user appointments.

-   **`GET /appointments`**
    -   Description: Retrieves appointments. Admins get all, clients get their own.
    -   Auth: Required.
    -   Response: `[{ "appointment_data" }]`

-   **`GET /appointments/{id}`**
    -   Description: Retrieves a single appointment by its ID.
    -   Auth: Required (Owner or Admin).
    -   Response: `{ "appointment_data" }`

-   **`POST /appointments`**
    -   Description: Creates a new appointment.
    -   Auth: Required.
    -   Body: `{ "serviceId": "ObjectId", "consultantId": "ObjectId", "scheduledAt": "Date", ... }`
    -   Response: `{ "appointment_data" }`

-   **`PUT /appointments/{id}`**
    -   Description: Updates an existing appointment (e.g., reschedule, update notes).
    -   Auth: Required (Owner or Admin).
    -   Body: `{ "scheduledAt": "Date", "notes": "String", ... }`
    -   Response: `{ "appointment_data" }`

---

## Services

Public and admin endpoints for managing services.

-   **`GET /services`**
    -   Description: Retrieves a list of all active services.
    -   Response: `[{ "service_data" }]`

-   **`GET /services/{id}`**
    -   Description: Retrieves a single service by its ID.
    -   Response: `{ "service_data" }`

-   **`POST /services`**
    -   Description: Creates a new service.
    -   Auth: Admin Required.
    -   Body: `{ "service_data" }`
    -   Response: `{ "insertedId": "ObjectId" }`

-   **`PUT /services/{id}`**
    -   Description: Updates an existing service.
    -   Auth: Admin Required.
    -   Body: `{ "service_data" }`
    -   Response: `{ "message": "String" }`

-   **`DELETE /services/{id}`**
    -   Description: Deletes a service.
    -   Auth: Admin Required.
    -   Response: `{ "message": "String" }`

---

## Scholarships

Public and admin endpoints for managing scholarships.

-   **`GET /scholarships`**
    -   Description: Retrieves a list of active scholarships. Supports filtering via query parameters (e.g., `?category=...&country=...`).
    -   Response: `[{ "scholarship_data" }]`

-   **`GET /scholarships/{id}`**
    -   Description: Retrieves a single scholarship by its ID.
    -   Response: `{ "scholarship_data" }`

-   **`POST /scholarships`**
    -   Description: Creates a new scholarship.
    -   Auth: Admin Required.
    -   Body: `{ "scholarship_data" }`
    -   Response: `{ "scholarship_data" }`

-   **`PUT /scholarships/{id}`**
    -   Description: Updates an existing scholarship.
    -   Auth: Admin Required.
    -   Body: `{ "scholarship_data" }`
    -   Response: `{ "scholarship_data" }`

-   **`DELETE /scholarships/{id}`**
    -   Description: Deactivates a scholarship (soft delete).
    -   Auth: Admin Required.
    -   Response: `{ "success": true }`

---

## Scholarship Favorites

Endpoints for users to manage their favorite scholarships.

-   **`GET /scholarship-favorites`**
    -   Description: Retrieves the list of favorite scholarships for the authenticated user.
    -   Auth: Required.
    -   Response: `[{ "scholarship_data" }]`

-   **`POST /scholarship-favorites`**
    -   Description: Toggles a scholarship as a favorite for the user.
    -   Auth: Required.
    -   Body: `{ "scholarshipId": "ObjectId" }`
    -   Response: `{ "favorited": "Boolean" }`

---

## Payments

Endpoints for handling payment processing.

-   **`GET /payments`**
    -   Description: Retrieves payment history. Admins get all, clients get their own.
    -   Auth: Required.
    -   Response: `[{ "payment_data" }]`

-   **`POST /payments`**
    -   Description: Initializes a payment transaction with the payment gateway (Paystack).
    -   Auth: Required.
    -   Body: `{ "serviceId": "ObjectId", "email": "String" }`
    -   Response: `{ "authorization_url": "String", "access_code": "String", "reference": "String" }`

---

## Webhooks

-   **`POST /paystack-webhook`**
    -   Description: Handles incoming webhook events from Paystack to update payment statuses.
    -   Auth: None (verified by Paystack signature).
    -   Response: `200 OK`
