# MongoDB Schema

This document outlines the MongoDB database schema for the ExploreWithRJ application.

## Collections

The database consists of the following collections:

-   [Users](#users)
-   [Appointments](#appointments)
-   [Services](#services)
-   [Scholarships](#scholarships)
-   [Scholarship Favorites](#scholarship-favorites)
-   [Payments](#payments)

---

### Users

Stores user account information and roles.

-   `_id`: **ObjectId** - Unique identifier for the user.
-   `fullName`: **String** - The user's full name.
-   `email`: **String** - The user's email address (unique, indexed).
-   `password`: **String** - Hashed password for the user.
-   `role`: **String** - Role of the user (e.g., `client`, `admin`, `consultant`). Indexed.
-   `phone`: **String** - User's phone number (optional).
-   `bio`: **String** - A short biography or description (optional).
-   `location`: **String** - User's country or city (optional).
-   `preferences`: **Object** - User-specific settings (optional).
-   `isActive`: **Boolean** - Whether the user account is active. Defaults to `true`.
-   `createdAt`: **Date** - Timestamp of when the user was created.
-   `updatedAt`: **Date** - Timestamp of the last update.

---

### Appointments

Stores information about scheduled appointments or consultations.

-   `_id`: **ObjectId** - Unique identifier for the appointment.
-   `clientId`: **ObjectId** - Reference to the `users` collection (_id of the client). Indexed.
-   `consultantId`: **ObjectId** - Reference to the `users` collection (_id of the consultant). Indexed.
-   `serviceId`: **ObjectId** - Reference to the `services` collection.
-   `scheduledAt`: **Date** - The date and time the appointment is scheduled for. Indexed.
-   `status`: **String** - The current status of the appointment (e.g., `pending`, `confirmed`, `completed`, `cancelled`). Indexed.
-   `notes`: **String** - Any notes or comments from the client for the appointment (optional).
-   `timezone`: **String** - The client's timezone for the appointment (e.g., "Africa/Lagos").
-   `createdAt`: **Date** - Timestamp of when the appointment was created.
-   `updatedAt`: **Date** - Timestamp of the last update.

---

### Services

Stores details about the services offered (e.g., consultation sessions, e-books).

-   `_id`: **ObjectId** - Unique identifier for the service.
-   `name`: **String** - The name of the service.
-   `description`: **String** - A detailed description of the service.
-   `shortDescription`: **String** - A brief summary of the service.
-   `price`: **Number** - The price of the service.
-   `type`: **String** - The type of service (e.g., `consultation`, `ebook`).
-   `category`: **String** - The category the service belongs to (e.g., "Career", "Education").
-   `durationMinutes`: **Number** - For consultations, the duration in minutes.
-   `features`: **[String]** - A list of key features or benefits of the service.
-   `selarLink`: **String** - For e-books, a link to the purchase page (e.g., on Selar).
-   `isActive`: **Boolean** - Whether the service is currently available.
-   `seoTitle`: **String** - SEO-optimized title for the service page.
-   `seoDescription`: **String** - SEO-optimized meta description.
-   `createdAt`: **Date** - Timestamp of when the service was created.
-   `updatedAt`: **Date** - Timestamp of the last update.

---

### Scholarships

Stores information about available scholarships.

-   `_id`: **ObjectId** - Unique identifier for the scholarship.
-   `title`: **String** - The title of the scholarship.
-   `description`: **String** - A detailed description of the scholarship.
-   `provider`: **String** - The organization or entity providing the scholarship.
-   `amount`: **Number** - The monetary value of the scholarship.
-   `deadline`: **Date** - The application deadline. Indexed.
-   `category`: **String** - The field or category of the scholarship (e.g., `Undergraduate`, `Masters`, `PhD`). Indexed.
-   `country`: **String** - The country where the scholarship is applicable. Indexed.
-   `fieldOfStudy`: **String** - The specific academic field the scholarship is for.
-   `isActive`: **Boolean** - Whether the scholarship is currently active and open for applications. Indexed.
-   `createdAt`: **Date** - Timestamp of when the scholarship was added.
-   `updatedAt`: **Date** - Timestamp of the last update.

---

### Scholarship Favorites

Tracks which scholarships a user has marked as a favorite.

-   `_id`: **ObjectId** - Unique identifier for the favorite entry.
-   `userId`: **ObjectId** - Reference to the `users` collection.
-   `scholarshipId`: **ObjectId** - Reference to the `scholarships` collection.
-   `createdAt`: **Date** - Timestamp of when the favorite was created.

A unique compound index exists on `(userId, scholarshipId)` to prevent duplicate entries.

---

### Payments

Stores records of payment transactions.

-   `_id`: **ObjectId** - Unique identifier for the payment.
-   `userId`: **ObjectId** - Reference to the `users` collection for the user who made the payment. Indexed.
-   `appointmentId`: **ObjectId** - Reference to an `appointments` entry, if applicable.
-   `serviceId`: **ObjectId** - Reference to the `services` collection for the purchased service.
-   `status`: **String** - The status of the payment (e.g., `pending`, `success`, `failed`). Indexed.
-   `amount`: **Number** - The amount paid.
-   `currency`: **String** - The currency of the payment (e.g., `NGN`).
-   `reference`: **String** - The unique transaction reference from the payment gateway (e.g., Paystack).
-   `createdAt`: **Date** - Timestamp of when the payment was initiated.
