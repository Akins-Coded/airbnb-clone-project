
# 🗂️ Database Requirements Specification

## 📘 Overview

This document defines the database schema for a property booking platform. It outlines the data entities, their attributes, constraints, and the relationships among them. The system enables users (guests and hosts) to book, manage, and review properties, with support for payments and messaging.

---

## 📋 Entities and Attributes

### 1. **User**
Represents all users of the platform (Guests, Hosts, and Admins).

| Attribute        | Type         | Constraints                          |
|------------------|--------------|--------------------------------------|
| `id`             | `objectId`   | Primary key                          |
| `first_name`     | `varchar`    | Not null                             |
| `last_name`      | `varchar`    | Not null                             |
| `email`          | `varchar`    | Unique, not null                     |
| `password_hash`  | `varchar`    | Not null                             |
| `phone_number`   | `varchar`    | Not null                             |
| `role`           | `user_role_enum` | Enum: `guest`, `host`, `admin`     |
| `created_at`     | `timestamptz`| Default: `now()`                     |

---

### 2. **Property**
Represents properties listed by users with a host role.

| Attribute        | Type         | Constraints                          |
|------------------|--------------|--------------------------------------|
| `id`             | `objectId`   | Primary key                          |
| `host_id`        | `objectId`   | Foreign key → `user.id`              |
| `name`           | `varchar`    | Not null                             |
| `description`    | `text`       | Not null                             |
| `location`       | `varchar`    | Not null                             |
| `pricepernight`  | `decimal`    | Not null                             |
| `created_at`     | `timestamptz`| Default: `now()`                     |
| `updated_at`     | `timestamptz`| Default: `now()`                     |

---

### 3. **Booking**
Captures booking records for properties.

| Attribute        | Type              | Constraints                          |
|------------------|-------------------|--------------------------------------|
| `id`             | `objectId`        | Primary key                          |
| `property_id`    | `objectId`        | Foreign key → `property.id`          |
| `user_id`        | `objectId`        | Foreign key → `user.id`              |
| `start_time`     | `timestamptz`     | Not null                             |
| `end_time`       | `timestamptz`     | Not null                             |
| `total_price`    | `decimal`         | Not null                             |
| `status`         | `bookings_status_enum` | Enum: `confirmed`, `pending`, `cancelled` |
| `created_at`     | `timestamptz`     | Default: `now()`                     |

---

### 4. **Payment**
Stores payment transactions for bookings.

| Attribute        | Type              | Constraints                          |
|------------------|-------------------|--------------------------------------|
| `id`             | `objectId`        | Primary key                          |
| `booking_id`     | `objectId`        | Foreign key → `booking.id`           |
| `amount`         | `decimal`         | Not null                             |
| `payment_method` | `payment_method_enum` | Enum: `credit_card`, `paypal`, `stripe` |
| `created_at`     | `timestamptz`     | Default: `now()`                     |

---

### 5. **Review**
Contains ratings and comments left by users on properties.

| Attribute        | Type         | Constraints                          |
|------------------|--------------|--------------------------------------|
| `id`             | `objectId`   | Primary key                          |
| `property_id`    | `objectId`   | Foreign key → `property.id`          |
| `rating`         | `smallint`   | Not null, Range: 1 to 5              |
| `comment`        | `text`       | Not null                             |
| `created_at`     | `timestamptz`| Default: `now()`                     |

---

### 6. **Message**
Allows users to send and receive messages on the platform.

| Attribute        | Type         | Constraints                          |
|------------------|--------------|--------------------------------------|
| `id`             | `objectId`   | Primary key                          |
| `sender_id`      | `objectId`   | Foreign key → `user.id`              |
| `recipient_id`   | `objectId`   | Foreign key → `user.id`              |
| `message_body`   | `text`       | Not null                             |
| `send_at`        | `timestamptz`| Default: `now()`                     |

---

## 🔗 Relationships

| Entity A   | Relationship           | Entity B   | Cardinality               |
|------------|------------------------|------------|---------------------------|
| User       | Hosts                  | Property   | One-to-Many (1:N)         |
| User       | Makes                  | Booking    | One-to-Many (1:N)         |
| Property   | Is Booked In           | Booking    | One-to-Many (1:N)         |
| Booking    | Has                    | Payment    | One-to-One (1:1)          |
| Property   | Has                    | Review     | One-to-Many (1:N)         |
| User       | Sends/Receives         | Message    | Many-to-Many (via user.id)|

---

## 📌 Enums

### `user_role_enum`
- `guest`
- `host`
- `admin`

### `bookings_status_enum`
- `confirmed`
- `pending`
- `cancelled`

### `payment_method_enum`
- `credit_card`
- `paypal`
- `stripe`

---

## ✅ Notes

- All `objectId` types represent unique identifiers, suitable for UUIDs or MongoDB-style IDs.
- Timestamps default to current UTC time using `now()`.
- Sensitive fields like `password_hash` must be securely stored and handled.
