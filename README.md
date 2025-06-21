# Airbnb Clone Project

## Overview
The Airbnb Clone Project is a web application designed to mimic the core functionalities of Airbnb, enabling users to browse, book, and manage accommodations online. This project aims to provide a seamless and user-friendly platform for both hosts and guests, facilitating easy listing of properties and hassle-free booking experiences.

## Project Goals
- Build a fully functional accommodation booking platform similar to Airbnb.
- Implement user authentication and profiles for hosts and guests.
- Enable hosts to create, update, and manage property listings.
- Allow guests to search for listings based on location, date, and other filters.
- Support booking management, including reservation creation and cancellation.
- Provide a clean, intuitive user interface with responsive design.
- Ensure secure handling of user data and transactions.


## Technology Stack

- **Python**: The primary programming language used to build the backend of the application, chosen for its simplicity and powerful libraries.

- **Django**: A high-level Python web framework that enables rapid development and clean, pragmatic design. It handles URL routing, database interaction, templating, and user authentication for the project.

- **Django REST Framework (DRF)**: A powerful and flexible toolkit for building Web APIs in Django. It facilitates creating RESTful endpoints to support frontend-backend communication.

- **djangorestframework-simplejwt**: A JSON Web Token (JWT) authentication plugin for Django REST Framework. It provides secure, stateless authentication for the API, managing user tokens for login sessions.

- **SQLite**: The default lightweight relational database used during development for storing user data, property listings, and bookings. Can be replaced with more robust databases like PostgreSQL for production.

- **HTML & CSS**: Markup and styling languages used to create the structure and design of the frontend pages.

- **JavaScript**: Adds interactivity and dynamic behavior to the frontend components, enhancing user experience.


## Getting Started

### Prerequisites
- Python 3.13 
- Django 

## Team Roles

### Backend Developer
Responsible for designing, implementing, and maintaining the server-side logic of the application. They develop APIs, handle data processing, and ensure the backend integrates smoothly with the frontend. In this project, they focus on building the Django application, managing user authentication, business logic, and booking workflows.

### Database Administrator (DBA)
Manages the database system, ensuring data integrity, security, and performance optimization. They design the database schema, oversee migrations, and perform backups. For this project, the DBA ensures that property listings, user data, and bookings are stored efficiently and securely.

### Frontend Developer
Creates the user interface and user experience by developing the client-side application. They work with HTML, CSS, and JavaScript to build responsive, accessible, and visually appealing web pages. In this project, they implement Django templates and integrate Bootstrap or custom CSS to enhance UI.

### DevOps Engineer
Handles deployment, server management, and continuous integration/continuous deployment (CI/CD) pipelines. They ensure the application runs smoothly in different environments and manage cloud services or hosting solutions. For this project, the DevOps Engineer configures the web server, manages dependencies, and sets up automated deployment workflows.

### QA Engineer (Quality Assurance)
Ensures the application meets quality standards by designing and running tests, including unit tests, integration tests, and user acceptance tests. They help identify bugs and verify that features work as expected. In this project, the QA Engineer tests booking flows, authentication, and overall usability.

### Project Manager
Oversees project planning, timelines, and team communication. They coordinate tasks, manage deadlines, and ensure the project aligns with the set goals. The Project Manager facilitates collaboration between developers, testers, and stakeholders to deliver the project successfully.

## Database Design

The project’s database is designed to capture essential data for managing users, properties, bookings, reviews, and payments. Below are the key entities, their important fields, and relationships:

### Entities and Fields

#### 1. Users
- `id` (Primary Key): Unique identifier for each user.
- `username`: User's unique login name.
- `email`: Contact email address.
- `password`: Hashed password for authentication.
- `is_host`: Boolean indicating if the user is a host or guest.
- `profile_photo`: Optional user profile image.

#### 2. Properties
- `id` (Primary Key): Unique identifier for each property listing.
- `host_id` (Foreign Key to Users): The user who owns/listed the property.
- `title`: Name or title of the property.
- `description`: Detailed description of the property.
- `location`: Address or geographical location.
- `price_per_night`: Rental price per night.

#### 3. Bookings
- `id` (Primary Key): Unique booking identifier.
- `property_id` (Foreign Key to Properties): The property being booked.
- `guest_id` (Foreign Key to Users): The user who made the booking.
- `start_date`: Booking start date.
- `end_date`: Booking end date.
- `total_price`: Calculated total price for the stay.

#### 4. Reviews
- `id` (Primary Key): Unique review identifier.
- `booking_id` (Foreign Key to Bookings): The booking this review is associated with.
- `reviewer_id` (Foreign Key to Users): The user who wrote the review.
- `rating`: Numeric rating (e.g., 1-5 stars).
- `comment`: Text feedback about the stay.

#### 5. Payments
- `id` (Primary Key): Unique payment identifier.
- `booking_id` (Foreign Key to Bookings): The booking this payment covers.
- `payment_date`: Date when the payment was made.
- `amount`: Amount paid.
- `payment_status`: Status of the payment (e.g., completed, pending).

### Entity Relationships

- **Users (1) --- (M) Properties**  
  A user who is a host can own multiple properties.  
  (Foreign Key: `Properties.host_id` → `Users.id`)

- **Users (1) --- (M) Bookings**  
  A user acting as a guest can make multiple bookings.  
  (Foreign Key: `Bookings.guest_id` → `Users.id`)

- **Properties (1) --- (M) Bookings**  
  Each booking is for one property.  
  (Foreign Key: `Bookings.property_id` → `Properties.id`)

- **Bookings (1) --- (1) Reviews**  
  Each booking can have one review, written by the guest who made the booking.  
  (Foreign Key: `Reviews.booking_id` → `Bookings.id`)

- **Bookings (1) --- (1) Payments**  
  Each booking has one associated payment record.  
  (Foreign Key: `Payments.booking_id` → `Bookings.id`)

- **Users and Properties** do **NOT** have a many-to-many relationship in this model (hosts own properties exclusively).

- There is no explicit many-to-many relationship currently defined. If features like user wishlists or property amenities were added, those would likely use M:N relationships.

---

This structure ensures clarity and integrity of the data while supporting the core Airbnb clone functionalities.

## Feature Breakdown

### User Management
This feature allows users to register, log in, and manage their profiles. It supports different user roles such as guests and hosts, enabling personalized experiences like managing property listings for hosts and booking stays for guests. Secure authentication ensures user data protection.

### Property Management
Hosts can create, update, and delete property listings through this feature. It includes detailed descriptions, pricing, and location information to help guests find suitable accommodations. The feature also supports managing availability to prevent double bookings.

### Booking System
The booking system enables guests to search for properties, select dates, and reserve stays. It manages booking statuses and ensures date availability, while calculating total costs. This feature is central to the platform’s purpose, facilitating smooth and reliable reservations.

### Reviews and Ratings
Guests can leave reviews and ratings for properties after their stay, providing valuable feedback for future users. This feature helps build trust within the community by promoting transparency and quality control.

### Payment Processing
This feature handles secure payment transactions associated with bookings. It tracks payment status and amounts, ensuring hosts receive payments and guests are charged correctly. Integration with secure payment gateways can be added to enhance this functionality.

### Responsive User Interface
The frontend delivers a clean, intuitive, and mobile-friendly experience. Using Django templates alongside HTML, CSS, and JavaScript, it ensures users can easily navigate the platform, search for listings, and complete bookings from any device.

## API Security

Ensuring the security of the Airbnb Clone's API is critical to protect user data, maintain trust, and prevent unauthorized access. Below are the key security measures implemented in the project, along with their importance:

### Authentication
We use **JWT (JSON Web Tokens)** provided by `djangorestframework-simplejwt` to authenticate users securely. Tokens are issued upon login and must be included in subsequent requests to access protected endpoints. This ensures that only verified users can access or manipulate their data.

> **Why it matters:** Prevents unauthorized users from accessing accounts or performing actions like creating bookings or editing listings.

### Authorization
Role-based access control (RBAC) is implemented to differentiate between guests and hosts. Hosts can create and manage properties, while guests can only book and review listings. Each API endpoint enforces permission checks to ensure appropriate access levels.

> **Why it matters:** Ensures that users can only perform actions that match their roles, preventing misuse of the system.

### Rate Limiting *(Optional / Future Implementation)*
Rate limiting can be configured to limit the number of API requests a user can make in a given timeframe. This helps prevent abuse, brute-force attacks, and reduces server load.

> **Why it matters:** Protects against denial-of-service (DoS) attacks and automated malicious activity.

### Data Validation and Sanitization
All incoming data is validated using Django REST Framework serializers to prevent injection attacks and data corruption. Fields like email, passwords, and dates are rigorously checked before saving.

> **Why it matters:** Ensures data integrity and protects the database from malformed or malicious inputs.

### HTTPS Enforcement *(Deployment Phase)*
All API traffic should be encrypted over HTTPS in production to prevent data interception. This will be enforced at the server level using SSL/TLS certificates.

> **Why it matters:** Protects sensitive information such as login credentials and payment details from being intercepted over insecure networks.

### Secure Payment Handling
Although the payment system in this project is a basic mock, integration with a secure third-party payment gateway (e.g., Stripe, PayPal) is recommended. These platforms handle sensitive financial data and offer built-in fraud protection.

> **Why it matters:** Ensures users' payment data is handled securely and reduces liability for the platform.

---

Together, these measures form a robust security layer that helps protect the system from common vulnerabilities and ensure a trustworthy platform for users and hosts.
