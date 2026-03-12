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

- Frontend   -->   React 19, TypeScript, Vite, Tailwind CSS v4, shadcn/ui.
- Backend    -->   Spring Boot 3, Spring Security, Spring Data JPA.
- Database   -->   PostgreSQL (Supabase).
- Authentication   -->   Clerk
- Caching    -->   Redis
- API Docs   -->   SpringDoc OpenAPI (Swagger UI)
- Frontend Deployment   -->   Vercel
- Backend Deployment    -->   Railway

## Getting Started (Local Development)

- Node.js 
- Java 
- Maven 3.9+
- PostgreSQL (Supabase)
- A Clerk account with a configured application

## Environment Variables

### Frontend 

- VITE_CLERK_PUBLISHABLE_KEY   -->   pk_test_a2V5LXBvc3N1bS03MS5jbGVyay5hY2NvdW50cy5kZXYk
- VITE_API_BASE_URL   -->   https://skill-mentor-backend-server-production.up.railway.app

###Backend

- CLERK_JWKS_URL   -->   https://key-possum-71.clerk.accounts.dev/.well-known/jwks.json
- CORS_ALLOWED_ORIGINS   -->   http://localhost:3001,https://skillmentor-frontend-eta.vercel.app
- DATABASE_URL   -->   jdbc:postgresql://aws-1-ap-southeast-2.pooler.supabase.com:5432/postgres
- DB_PASSWORD   -->   En8xdyw5ebSY8Tw0
- DB_USERNAME   -->   postgres.zzaxyilahgcqllvlyuux

## API Documentation

#### Mentors
### Method         ### Endpoint                      ### Description
- GET    --->   /api/v1/mentors        --->    PublicList all mentors (paginated). Optional ?name= query param for search.
- GET    --->   /api/v1/mentors/{id}   --->    PublicGet a single mentor by ID.
- GET    --->   /api/v1/mentors/{id}/profile   --->    PublicRich mentor profile with stats, per-subject enrollment counts, and reviews.
- POST   --->   /api/v1/mentors        --->   ADMIN / MENTORCreate a new mentor profile.
- PUT    --->   /api/v1/mentors/{id}   --->   ADMIN / MENTORUpdate mentor profile.
- DELETE --->   /api/v1/mentors/{id}   --->    ADMINDelete a mentor.

### Subjects
### Method       ### Endpoint              ### Description
GET     --->   /api/v1/subjects       --->  AnyList all subjects
GET     --->   /api/v1/subjects/{id}  --->  AnyGet subject by ID
POST    --->   /api/v1/subjects       --->  ADMIN / MENTORCreate a subject and assign to a mentor
PUT     --->   /api/v1/subjects/{id}  --->  ADMIN / MENTORUpdate a subject
DELETE  --->   /api/v1/subjects/{id}  --->  ADMINDelete a subject
