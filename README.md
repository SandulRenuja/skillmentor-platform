# SkillMentor 

Students can browse mentors, view profiles, book sessions, upload payment proof, and track their learning progress.
Admins can manage mentors, subjects, and bookings through a dedicated dashboard — all without touching Postman.

## 1.Features

### Student

- Browse all available mentors with search and pagination
- View rich mentor profile pages (bio, subjects, stats, reviews)
- Book one-on-one sessions with date/time selection
- *Subject selection when a mentor teaches multiple subjects
- Past-date booking prevention (client + server enforced)
- Double-booking prevention no overlapping slots for the same mentor or student
- Upload payment proof (bank slip) after booking
- View enrolled sessions in a personal dashboard
- Track session status (scheduled → confirmed → completed)
- Write a star rating + review on completed sessions

### Admin

- Role-based access control via Clerk public metadata
- Create mentors with full profile information via a form
- Create subjects and assign them to mentors
- View all bookings in a data table with filters
- Confirm pending payments
- Mark sessions as completed
Add meeting links to confirmed sessions

### Platform

- Clerk JWT authentication (Clerk-issued tokens verified on the backend)
- Public mentor discovery (no login required to browse)
- Redis caching for mentor listings
- OpenAPI / Swagger documentation
- CORS configuration for cross-origin frontend/backend deployment
- Dual auth validator support (Clerk JWKS or custom HS256 JWT)

## Tech Stack
