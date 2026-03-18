# SkillMentor 

Students can browse mentors, view profiles, book sessions, upload payment proof, and track their learning progress.
Admins can manage mentors, subjects, and bookings through a dedicated dashboard.

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

| Layer | Technology |
| :--- | :--- |
| **Frontend** | React 19, TypeScript, Vite, Tailwind CSS v4, shadcn/ui |
| **Backend** | Spring Boot 3, Spring Security, Spring Data JPA |
| **Database** | PostgreSQL (Supabase) |
| **Authentication** | Clerk |
| **Caching** | Redis |
| **API Documentation** | SpringDoc OpenAPI (Swagger UI) |
| **Frontend Hosting** | Vercel |
| **Backend Hosting** | Railway (Serverless Mode) |

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

### Backend

- CLERK_JWKS_URL   
- CORS_ALLOWED_ORIGINS   
- DATABASE_URL  
- DB_PASSWORD   
- DB_USERNAME   

## API Documentation

#### Mentors

| Method | Endpoint | Permission | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/mentors` | Public | List all mentors (paginated). Supports `?name=` search. |
| `GET` | `/api/v1/mentors/{id}` | Public | Get a single mentor by ID |
| `GET` | `/api/v1/mentors/{id}/profile` | Public | Rich mentor profile with stats and reviews |
| `POST` | `/api/v1/mentors` | ADMIN / MENTOR | Create a new mentor profile |
| `PUT` | `/api/v1/mentors/{id}` | ADMIN / MENTOR | Update mentor profile |
| `DELETE` | `/api/v1/mentors/{id}` | ADMIN | Delete a mentor |

### Subjects

| Method | Endpoint | Permission | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/subjects` | Any | List all subjects |
| `GET` | `/api/v1/subjects/{id}` | Any | Get subject by ID |
| `POST` | `/api/v1/subjects` | ADMIN / MENTOR | Create a subject and assign to a mentor |
| `PUT` | `/api/v1/subjects/{id}` | ADMIN / MENTOR | Update a subject |
| `DELETE` | `/api/v1/subjects/{id}` | ADMIN | Delete a subject |

### Sessions

| Method | Endpoint | Permission | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/sessions` | ADMIN | List all sessions across the platform |
| `GET` | `/api/v1/sessions/{id}` | ADMIN | Get a single session by ID |
| `POST` | `/api/v1/sessions` | ADMIN | Create a session manually |
| `PUT` | `/api/v1/sessions/{id}` | ADMIN | Update a session (status, link, etc.) |
| `DELETE` | `/api/v1/sessions/{id}` | ADMIN | Delete a session |
| `POST` | `/api/v1/sessions/enroll` | STUDENT / ADMIN | Enroll a student (booking flow) |
| `GET` | `/api/v1/sessions/my-sessions` | STUDENT / ADMIN | Get authenticated student's sessions |
| `PATCH` | `/api/v1/sessions/{id}/review` | STUDENT / ADMIN | Submit a star rating and review |

### Students

| Method | Endpoint | Permission | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/students` | Any Authenticated | List all students |
| `GET` | `/api/v1/students/{id}` | Any Authenticated | Get student by ID |
| `POST` | `/api/v1/students` | ADMIN / STUDENT | Create a student profile |
| `PUT` | `/api/v1/students/{id}` | ADMIN / STUDENT | Update student profile |
| `DELETE` | `/api/v1/students/{id}` | ADMIN | Delete a student |

### Deployed Links:

| Environment | Status | URL |
| :--- | :--- | :--- |
| **Frontend** | Live | [skillmentor-frontend-eta.vercel.app](https://skillmentor-frontend-eta.vercel.app/) |
| **Backend** | Active | [skill-mentor-backend-server-production.up.railway.app](https://skill-mentor-backend-server-production.up.railway.app) |

### Project Structure

### Backend Structure
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
```

### Frontend Structure
```text
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
```
