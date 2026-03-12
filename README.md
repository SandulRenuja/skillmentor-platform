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

- CLERK_JWKS_URL   
- CORS_ALLOWED_ORIGINS   
- DATABASE_URL  
- DB_PASSWORD   
- DB_USERNAME   

## API Documentation

#### Mentors

- GET    --->   /api/v1/mentors        --->    PublicList all mentors (paginated). Optional ?name= query param for search.
- GET    --->   /api/v1/mentors/{id}   --->    PublicGet a single mentor by ID.
- GET    --->   /api/v1/mentors/{id}/profile   --->    PublicRich mentor profile with stats, per-subject enrollment counts, and reviews.
- POST   --->   /api/v1/mentors        --->   ADMIN / MENTORCreate a new mentor profile.
- PUT    --->   /api/v1/mentors/{id}   --->   ADMIN / MENTORUpdate mentor profile.
- DELETE --->   /api/v1/mentors/{id}   --->    ADMINDelete a mentor.

### Subjects

- GET     --->   /api/v1/subjects       --->  AnyList all subjects
- GET     --->   /api/v1/subjects/{id}  --->  AnyGet subject by ID
- POST    --->   /api/v1/subjects       --->  ADMIN / MENTORCreate a subject and assign to a mentor
- PUT     --->   /api/v1/subjects/{id}  --->  ADMIN / MENTORUpdate a subject
- DELETE  --->   /api/v1/subjects/{id}  --->  ADMINDelete a subject

### Sessions

- GET     --->   /api/v1/sessions        --->   ADMIN  --->  List all sessions across the platform
- GET     --->   /api/v1/sessions/{id}   --->  ADMIN --->  Get a single session by ID
- POST    --->   /api/v1/sessions        --->  ADMIN --->  Create a session manually
- PUT     --->   /api/v1/sessions/{id}   --->  ADMIN --->  Update a session (status, meeting link, etc.)
- DELETE  --->   /api/v1/sessions/{id}   --->  ADMIN --->  Delete a session
- POST    --->   /api/v1/sessions/enroll --->  STUDENT / ADMIN  --->  Enroll a student in a session (booking flow)
- GET     --->   /api/v1/sessions/my-sessions --->  STUDENT / ADMIN --->  Get the authenticated student's sessions
- PATCH   --->   /api/v1/sessions/{id}/review  --->STUDENT / ADMIN --->--->Submit a star rating and review on a completed session\

### Students
- GET     --->   /api/v1/students        --->  Any authenticated --->  List all students.
- GET     --->   /api/v1/students/{id}   --->  Any authenticated ---> Get student by ID
- POST    --->   /api/v1/students        --->  ADMIN / STUDENT  --->  Create a student profile
- PUT     --->   /api/v1/students/{id}   --->  ADMIN / STUDENT  --->  Update student profile
- DELETE  --->   /api/v1/students/{id}   --->  ADMIN  ---> Delete a student

### Deployed Links:

- Frontend   --->   https://skillmentor-frontend-eta.vercel.app/
- Backend   --->   https://skill-mentor-backend-server-production.up.railway.app

### Project Structure
```text
├── backend/                          # Spring Boot application
│   ├── src/main/java/com/stemlink/skillmentor/
│   │   ├── configs/                  # CORS, Security, Redis, ModelMapper, OpenAPI
│   │   │   ├── CorsConfig.java
│   │   │   ├── SecurityConfig.java
│   │   │   ├── RedisConfig.java
│   │   │   ├── ModelMapperConfig.java
│   │   │   ├── OpenApiConfig.java
│   │   │   └── ValidatorConfiguration.java
│   │   ├── constants/
│   │   │   └── UserRoles.java
│   │   ├── controllers/              # REST controllers
│   │   │   ├── AbstractController.java
│   │   │   ├── MentorController.java
│   │   │   ├── SessionController.java
│   │   │   ├── StudentController.java
│   │   │   └── SubjectController.java
│   │   ├── dto/                      # Data Transfer Objects
│   │   │   ├── MentorDTO.java
│   │   │   ├── SessionDTO.java
│   │   │   ├── StudentDTO.java
│   │   │   ├── SubjectDTO.java
│   │   │   ├── ReviewDTO.java
│   │   │   ├── ErrorResponse.java
│   │   │   └── response/
│   │   │       ├── SessionResponseDTO.java
│   │   │       ├── AdminSessionResponseDTO.java
│   │   │       └── MentorProfileResponseDTO.java
│   │   ├── entities/                 # JPA entities
│   │   │   ├── Mentor.java
│   │   │   ├── Student.java
│   │   │   ├── Subject.java
│   │   │   └── Session.java
│   │   ├── exceptions/
│   │   │   └── SkillMentorException.java
│   │   ├── repositories/
│   │   │   ├── MentorRepository.java
│   │   │   ├── SessionRepository.java
│   │   │   ├── StudentRepository.java
│   │   │   └── SubjectRepository.java
│   │   ├── security/                 # JWT validation & Spring Security filter
│   │   │   ├── AuthenticationFilter.java
│   │   │   ├── ClerkValidator.java
│   │   │   ├── SkillMentorJwtValidator.java
│   │   │   ├── SkillMentorAuthenticationEntryPoint.java
│   │   │   ├── TokenValidator.java
│   │   │   └── UserPrincipal.java
│   │   ├── services/
│   │   │   ├── MentorService.java
│   │   │   ├── SessionService.java
│   │   │   ├── StudentService.java
│   │   │   ├── SubjectService.java
│   │   │   └── impl/
│   │   │       ├── MentorServiceImpl.java
│   │   │       ├── SessionServiceImpl.java
│   │   │       ├── StudentServiceImpl.java
│   │   │       └── SubjectServiceImpl.java
│   │   └── utils/
│   │       └── ValidationUtils.java
│   ├── src/main/resources/
│   │   ├── application.properties
│   │   ├── application-dev.properties
│   │   └── application-prod.properties
│   ├── Dockerfile
│   ├── docker-compose.yaml
│   └── pom.xml
└─ frontend/                         # React + Vite application
    ├── src/
    │   ├── assets/                   # Images and static files
    │   ├── components/
    │   │   ├── ui/                   # shadcn/ui primitives
    │   │   │   ├── button.tsx
    │   │   │   ├── card.tsx
    │   │   │   ├── dialog.tsx
    │   │   │   ├── calendar.tsx
    │   │   │   ├── input.tsx
    │   │   │   ├── label.tsx
    │   │   │   ├── sheet.tsx
    │   │   │   ├── alert.tsx
    │   │   │   └── toast.tsx
    │   │   ├── Admin/
    │   │   │   └── AdminLayout.tsx   # Admin sidebar + role guard
    │   │   ├── Layout.tsx
    │   │   ├── Navigation.tsx
    │   │   ├── Footer.tsx
    │   │   ├── MentorCard.tsx        # Mentor listing card (links to profile)
    │   │   ├── SchedulingModal.tsx   # Date/time/subject booking modal
    │   │   ├── SignUpDialog.tsx      # Unauthenticated prompt
    │   │   ├── StatusPill.tsx        # Session/payment status badge
    │   │   └── WriteReviewDialog.tsx # Star rating + review submission
    │   ├── hooks/
    │   │   └── use-toast.ts
    │   ├── lib/
    │   │   └── utils.ts
    │   ├── pages/
    │   │   ├── HomePage.tsx          # Mentor discovery / browsing
    │   │   ├── LoginPage.tsx
    │   │   ├── DashboardPage.tsx     # Student session dashboard
    │   │   ├── PaymentPage.tsx       # Bank slip upload
    │   │   ├── MentorProfilePage.tsx # Rich mentor profile
    │   │   └── Admin/
    │   │       ├── AdminOverviewPage.tsx
    │   │       ├── CreateMentorPage.tsx
    │   │       ├── CreateSubjectPage.tsx
    │   │       └── ManageBookingsPage.tsx
    │   ├── types.ts                  # Shared TypeScript interfaces
    │   ├── App.tsx                   # Router + route definitions
    │   ├── main.tsx                  # Entry point + ClerkProvider
    │   └── index.css                 # Tailwind v4 + CSS design tokens
    ├── index.html
    ├── vite.config.ts
    ├── tsconfig.json
    ├── components.json               # shadcn/ui config
    └── vercel.json                   # SPA rewrite rule

### DDDDD

