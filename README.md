# TalentExchange 🚀

**TalentExchange** is a full-stack web platform developed to facilitate skill exchange based on a time-banking model. Users can act as both students (*Learners*) and teachers (*Mentors*), provide lessons, manage an availability calendar, and exchange meritocratic feedback using a virtual balance of time credits.

---

## 🛠️ Tech Stack

### Frontend
* **React (v18+)** with **TypeScript** for strict component typing and lifecycle management.
* **React Router (v6)** for client-side routing and private endpoint protection (`ProtectedRoute`).
* **Bootstrap 5** & **React-Bootstrap** for a responsive and accessible user interface.
* **Context API** for reactive global state management (user session, credit balance, active role).

### Backend
* **Java Spring Boot 3** (RESTful MVC Architecture).
* **Spring Data JPA / Hibernate** for ORM and relational data persistence.
* **Custom Authentication Management:** Dedicated servlet filters (`AuthFilter`), thread-safe in-memory sessions with *Sliding Expiration* (`SessionManager`), and `HttpOnly` cookie support to mitigate XSS vulnerabilities.
* **Transactional Management (`@Transactional`):** Atomic execution of transactions on time credits and prevention of *Race Conditions* (Overbooking).

---

## ✨ Architectural Features

1. **Identity Management & Dynamic Roles:**
   * Authentication based on secure session cookies.
   * **Role Switch:** Standard users can dynamically toggle their operational interface between *Learner* and *Mentor*.
   * Administration panel (`ADMIN`) for global moderation of users, ads, transactions, and reviews.

2. **Booking Engine & Collision Prevention:**
   * Integrated scheduling modal with a calendar for time slot selection.
   * The availability algorithm (`AvailabilityService`) calculates 60-minute slots by dynamically cross-referencing the mentor's weekly rules with queued bookings, effectively excluding overlaps.
   * **Credit Escrow:** Preemptive credit deduction upon booking request (`PENDING`) and automatic rollback in case of rejection or cancellation.

3. **Skills Catalog & Search:**
   * Interactive search engine with cascading filters for macro-categories, specific subjects, and text search.
   * Hierarchical domain management (Category -> Skill -> Ad).

4. **Reputation & Feedback:**
   * Review system strictly tied to completed bookings, ensuring the integrity of the rating system (1-5 stars).

---

## 📂 Project Structure

```text
talent-exchange/
├── backend/                  # Spring Boot source code (Java)
│   ├── controller/           # REST Endpoints (Auth, Ads, Bookings, Availability, Admin)
│   ├── service/              # Core business logic and transactional algorithms
│   ├── repository/           # JPA Interfaces for database access
│   ├── entity/               # Domain models (User, Ad, Booking, Review, etc.)
│   └── config/               # Security interceptors and CORS policies
│
└── frontend/                 # React source code (TypeScript)
    ├── src/components/       # UI Components (Cards, Modals, Tables)
    ├── src/context/          # State management (AuthContext)
    ├── src/pages/            # View controllers
    ├── src/services/         # HTTP Client APIs
    └── src/types/            # TypeScript DTOs and Interfaces
