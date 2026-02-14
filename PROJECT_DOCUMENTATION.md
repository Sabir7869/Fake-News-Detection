# Fake News Detection System - Complete Project Documentation

> **Purpose**: Comprehensive documentation for portfolio showcase and resume writing.

---

## 📋 Table of Contents

1. [Project Overview](#-project-overview)
2. [Technical Architecture](#-technical-architecture)
3. [Technology Stack](#-technology-stack)
4. [Key Features](#-key-features)
5. [Database Design](#-database-design)
6. [API Design](#-api-design)
7. [AI/ML Integration](#-aiml-integration)
8. [Security Implementation](#-security-implementation)
9. [Frontend Architecture](#-frontend-architecture)
10. [Resume-Ready Points](#-resume-ready-points)
11. [Challenges & Solutions](#-challenges--solutions)
12. [Future Enhancements](#-future-enhancements)

---

## 🎯 Project Overview

### Summary
A full-stack web application that combats misinformation by providing AI-powered fact-checking with community-driven feedback. Users can submit news headlines, claims, or statements and receive intelligent verification results combining multiple AI sources with crowd-sourced validation.

### Problem Statement
In the digital age, misinformation spreads rapidly across social media and news outlets. This application addresses the critical need for accessible, reliable fact-checking tools that combine artificial intelligence with human collective intelligence.

### Solution
An intelligent verification system that:
- Aggregates data from Google Fact Check API, Custom Search API, and Google Gemini AI
- Implements a hybrid confidence scoring algorithm (70% AI + 30% community feedback)
- Provides clear, actionable verdicts: TRUE, FALSE, PARTIALLY_TRUE, UNVERIFIABLE

### Target Users
- Social media users wanting to verify viral content
- Journalists and researchers fact-checking sources
- General public seeking reliable news verification

---

## 🏗 Technical Architecture

### System Architecture
```
┌─────────────────────────────────────────────────────────────────────┐
│                         Frontend (Next.js)                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌────────────┐ │
│  │  Dashboard  │  │   Login/    │  │   History   │  │  Landing   │ │
│  │    Page     │  │  Register   │  │    Page     │  │    Page    │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └────────────┘ │
│         │                │                │                         │
│  ┌──────┴────────────────┴────────────────┴──────────────────────┐ │
│  │                    API Service Layer (lib/api.js)             │ │
│  │              - JWT Token Management                            │ │
│  │              - HTTP Request Handling                           │ │
│  └───────────────────────────┬───────────────────────────────────┘ │
└──────────────────────────────┼─────────────────────────────────────┘
                               │ REST API (JSON)
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     Backend (Spring Boot)                           │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                 Security Layer (JWT + BCrypt)                │   │
│  │    JwtAuthenticationFilter → SecurityFilterChain → CORS      │   │
│  └───────────────────────────┬─────────────────────────────────┘   │
│                              ▼                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                    REST Controllers                          │   │
│  │   UserController │ MessageController │ FeedbackController    │   │
│  └───────────────────────────┬─────────────────────────────────┘   │
│                              ▼                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                    Service Layer                             │   │
│  │  UserService │ MessageService │ FeedbackService │ VerifyServ │   │
│  └───────────────────────────┬─────────────────────────────────┘   │
│                              ▼                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                 Repository Layer (JPA)                       │   │
│  │   UserRepository │ MessageRepository │ FeedbackRepository    │   │
│  └───────────────────────────┬─────────────────────────────────┘   │
└──────────────────────────────┼─────────────────────────────────────┘
                               │
         ┌─────────────────────┼─────────────────────┐
         ▼                     ▼                     ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────┐
│   PostgreSQL    │  │  Google APIs    │  │     Google Gemini       │
│    Database     │  │  (Fact Check +  │  │    (AI Analysis)        │
│                 │  │  Custom Search) │  │                         │
└─────────────────┘  └─────────────────┘  └─────────────────────────┘
```

### Data Flow
1. **User submits claim** → Frontend validates and sends to backend
2. **Backend receives claim** → Checks cache (existing verification)
3. **If new claim** → Calls verification pipeline:
   - Google Fact Check API (existing fact-checks)
   - Google Custom Search API (relevant web results)
   - Google Gemini AI (intelligent analysis)
4. **Results aggregated** → Verdict + confidence score generated
5. **Response returned** → Frontend displays with community feedback options
6. **Community votes** → Dynamic confidence recalculation

---

## 🛠 Technology Stack

### Backend
| Technology | Version | Purpose |
|------------|---------|---------|
| **Java** | 21 | Core programming language |
| **Spring Boot** | 3.3.5 | Application framework |
| **Spring Security** | 6.x | Authentication & authorization |
| **Spring Data JPA** | 3.x | Database ORM |
| **Spring WebFlux** | 6.x | Reactive web client for API calls |
| **PostgreSQL** | 15+ | Relational database |
| **Lombok** | 1.18.x | Boilerplate code reduction |
| **JWT (jjwt)** | 0.11.x | Token-based authentication |
| **BCrypt** | - | Password hashing |
| **Maven** | 3.9.x | Build tool & dependency management |

### Frontend
| Technology | Version | Purpose |
|------------|---------|---------|
| **Next.js** | 16.1.6 | React framework with SSR/SSG |
| **React** | 19.2.3 | UI component library |
| **Tailwind CSS** | 4.x | Utility-first CSS framework |
| **ESLint** | 9.x | Code linting |
| **PostCSS** | 4.x | CSS processing |

### External APIs
| API | Purpose |
|-----|---------|
| **Google Fact Check API** | Access existing fact-check database |
| **Google Custom Search API** | Web search for supporting evidence |
| **Google Gemini 2.5 Flash** | AI-powered content analysis |

### DevOps & Tools
- Git for version control
- VS Code / IntelliJ IDEA for development
- Postman for API testing

---

## ✨ Key Features

### 1. AI-Powered Fact Verification
- **Multi-source verification**: Combines Fact Check API, Custom Search, and Gemini AI
- **Intelligent prompt engineering**: Structured prompts for consistent AI responses
- **Graceful degradation**: Fallback handling if any API fails

### 2. Hybrid Confidence Scoring
```
Final Confidence = (AI Confidence × 0.70) + (Community Score × 0.30)
```
- AI provides initial scientific assessment
- Community feedback adds human validation layer
- Real-time score updates as users vote

### 3. User Authentication & Authorization
- JWT-based stateless authentication
- BCrypt password encryption
- Protected routes with role-based access
- 24-hour token expiration with auto-refresh handling

### 4. Community Feedback System
- Like/dislike mechanism for verification results
- One vote per user per verification (updates allowed)
- Real-time statistics: total likes, dislikes, percentage

### 5. Verification History
- Local storage persistence for recent verifications
- Quick access to past fact-checks
- Delete capability for message cleanup

### 6. Verdicts System
| Verdict | Description |
|---------|-------------|
| **TRUE** | Claim verified as accurate |
| **FALSE** | Claim determined to be false |
| **PARTIALLY_TRUE** | Claim contains some truth but is misleading |
| **UNVERIFIABLE** | Insufficient evidence to determine accuracy |
| **PENDING** | Verification in progress or failed |

### 7. Responsive UI/UX
- Mobile-first responsive design
- Dark/light mode support
- Loading states with animations
- Toast notifications for user feedback
- Expandable AI summaries

---

## 🗄 Database Design

### Entity Relationship Diagram
```
┌─────────────────────┐       ┌─────────────────────┐
│       users         │       │      messages       │
├─────────────────────┤       ├─────────────────────┤
│ id (PK) BIGINT      │◄──┐   │ id (PK) BIGINT      │
│ name VARCHAR        │   │   │ content TEXT        │
│ email VARCHAR(UQ)   │   │   │ verdict VARCHAR     │
│ password VARCHAR    │   └───│ author_id (FK)      │
│ created_at DATE     │       │ confidence INT      │
└─────────────────────┘       │ summary TEXT        │
         │                    │ created_at DATE     │
         │                    └─────────────────────┘
         │                              │
         │    ┌─────────────────────────┘
         │    │
         ▼    ▼
┌─────────────────────┐
│     feedbacks       │
├─────────────────────┤
│ id (PK) BIGINT      │
│ user_id (FK)        │
│ message_id (FK)     │
│ liked BOOLEAN       │
│ date DATE           │
└─────────────────────┘
(user_id, message_id) = UNIQUE
```

### Table Specifications

#### users
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Unique identifier |
| name | VARCHAR(255) | NOT NULL | User's display name |
| email | VARCHAR(255) | UNIQUE, NOT NULL | Login credential |
| password | VARCHAR(255) | NOT NULL | BCrypt hashed |
| created_at | DATE | DEFAULT NOW() | Registration date |

#### messages
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Unique identifier |
| content | TEXT | NOT NULL | Original claim text |
| verdict | VARCHAR(50) | - | TRUE/FALSE/PARTIALLY_TRUE/UNVERIFIABLE/PENDING |
| confidence | INTEGER | 0-100 | AI confidence score |
| summary | TEXT | - | AI-generated explanation |
| author_id | BIGINT | FK → users.id | User who submitted |
| created_at | DATE | DEFAULT NOW() | Submission date |

#### feedbacks
| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | BIGINT | PK, AUTO_INCREMENT | Unique identifier |
| user_id | BIGINT | FK → users.id | Voter |
| message_id | BIGINT | FK → messages.id | Verification |
| liked | BOOLEAN | NOT NULL | true=like, false=dislike |
| date | DATE | DEFAULT NOW() | Vote date |
| - | - | UNIQUE(user_id, message_id) | One vote per user |

---

## 🌐 API Design

### Base URL
```
http://localhost:8080/api/v1
```

### Authentication
```http
Authorization: Bearer <JWT_TOKEN>
```

### Endpoints Summary

#### User Endpoints
| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/users/register` | ❌ | Register new user |
| POST | `/users/login` | ❌ | Login and get JWT |
| GET | `/users/{email}` | ✅ | Get user by email |
| GET | `/users/id/{userId}` | ✅ | Get user by ID |

#### Message Endpoints
| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/messages/verify` | ✅ | Submit claim for verification |
| GET | `/messages/{id}` | ✅ | Get verification by ID |
| DELETE | `/messages/{id}` | ✅ | Delete verification |
| GET | `/messages/{id}/confidence` | ✅ | Get dynamic confidence |

#### Feedback Endpoints
| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/feedbacks` | ✅ | Submit like/dislike |
| GET | `/feedbacks/stats/{messageId}` | ✅ | Get feedback statistics |
| GET | `/feedbacks/user/{userId}` | ✅ | Get user's feedbacks |
| GET | `/feedbacks/status` | ✅ | Check user's vote status |

### Request/Response Examples

#### Verify Claim
```http
POST /api/v1/messages/verify
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
Content-Type: application/json

{
  "content": "COVID-19 vaccines contain microchips",
  "userId": 1
}
```

```json
{
  "id": 42,
  "content": "COVID-19 vaccines contain microchips",
  "verdict": "FALSE",
  "confidence": 95,
  "summary": "This claim has been widely debunked. COVID-19 vaccines contain mRNA or viral vector components but no microchips. Multiple fact-checking organizations have verified this is misinformation.",
  "authorName": "John Doe",
  "authorEmail": "john@example.com",
  "createdAt": "2026-02-15",
  "feedBackStatsDTO": {
    "messageId": 42,
    "totalLikes": 150,
    "totalDislikes": 5,
    "likePercent": 96.77
  }
}
```

---

## 🤖 AI/ML Integration

### Verification Pipeline
```
User Input
    │
    ▼
┌─────────────────────────────────────────────────────────┐
│               VerificationService.verify()               │
└─────────────────────────────────────────────────────────┘
    │
    ├────────────────┬────────────────┬────────────────────┤
    ▼                ▼                ▼                    │
┌────────┐    ┌────────────┐    ┌──────────┐              │
│ Fact   │    │ Custom     │    │ Gemini   │◄─────────────┘
│ Check  │    │ Search     │    │ AI       │  (receives all data)
│ API    │    │ API        │    │          │
└────┬───┘    └─────┬──────┘    └────┬─────┘
     │              │                │
     │              │                ▼
     │              │         ┌─────────────┐
     └──────────────┴────────►│  Aggregated │
                              │   Result    │
                              └─────────────┘
```

### Google Fact Check API Integration
- Queries existing fact-check database
- Returns publisher, claim review, text rating
- Trusted fact-checking organizations only

### Google Custom Search API Integration
- Searches web for relevant articles
- Filters credible news sources
- Provides supporting evidence links

### Google Gemini AI Prompt Engineering
```
You are a professional fact-checking AI.

Analyze the claim using the evidence below.

CLAIM:
[User's submitted claim]

FACT CHECK DATA:
[Results from Fact Check API]

SEARCH RESULTS:
[Results from Custom Search API]

Respond ONLY in valid JSON:
{
  "verdict": "TRUE or FALSE or PARTIALLY_TRUE or UNVERIFIABLE",
  "confidence": 0-100,
  "summary": "brief explanation"
}
```

### Confidence Score Algorithm
```java
// AI contributes 70% to final score
double aiConfidence = message.getConfidence() / 100.0;

// Community feedback contributes 30%
double userScore = stats.getLikePercent() / 100.0;

// Final weighted calculation
double finalConfidence = ((aiConfidence * 0.7) + (userScore * 0.3)) * 100;
```

---

## 🔐 Security Implementation

### JWT Authentication Flow
```
┌──────────┐     POST /login      ┌──────────┐
│  Client  │ ──────────────────►  │  Server  │
│          │   {email, password}  │          │
│          │                      │          │
│          │  ◄────────────────── │          │
│          │   {user, token}      │          │
└──────────┘                      └──────────┘
     │
     │ Store JWT in localStorage
     ▼
┌──────────┐   GET /messages/*    ┌──────────┐
│  Client  │ ──────────────────►  │  Server  │
│          │   Authorization:     │          │
│          │   Bearer <token>     │          │
│          │                      │          │
│          │  ◄────────────────── │          │
│          │   Protected Data     │          │
└──────────┘                      └──────────┘
```

### Security Configuration
```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {
    
    // Stateless session management
    .sessionManagement(session -> 
        session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
    
    // Public endpoints
    .requestMatchers("/api/v1/users/register", "/api/v1/users/login").permitAll()
    
    // Protected endpoints
    .requestMatchers("/api/v1/users/**").authenticated()
    .requestMatchers("/api/v1/messages/**").authenticated()
    .requestMatchers("/api/v1/feedbacks/**").authenticated()
    
    // JWT filter before username/password auth
    .addFilterBefore(jwtAuthenticationFilter, 
                     UsernamePasswordAuthenticationFilter.class)
}
```

### Security Features
| Feature | Implementation |
|---------|----------------|
| Password Hashing | BCrypt with default strength |
| Token Algorithm | HS256 (HMAC-SHA256) |
| Token Expiration | 24 hours |
| CSRF Protection | Disabled (stateless API) |
| CORS | Enabled for all origins |
| Session | Stateless |

---

## 💻 Frontend Architecture

### Component Structure
```
src/
├── app/                    # Next.js App Router
│   ├── layout.js          # Root layout with providers
│   ├── page.js            # Landing page
│   ├── dashboard/
│   │   └── page.js        # Main verification interface
│   ├── history/
│   │   └── page.js        # Verification history
│   ├── login/
│   │   └── page.js        # Login form
│   └── register/
│       └── page.js        # Registration form
│
├── components/             # Reusable UI components
│   ├── Navbar.jsx         # Navigation bar
│   ├── VerificationForm.jsx    # Claim input form
│   ├── ResultCard.jsx     # Verification result display
│   ├── VerdictBadge.jsx   # Verdict indicator (TRUE/FALSE/etc)
│   ├── ConfidenceBar.jsx  # Visual confidence meter
│   ├── FeedbackButtons.jsx # Like/dislike buttons
│   └── Toast.jsx          # Notification system
│
├── context/
│   └── AuthContext.js     # Global authentication state
│
└── lib/
    └── api.js             # Backend API service layer
```

### State Management
- **React Context API** for global auth state
- **localStorage** for token persistence and history
- **useState/useEffect** for component-level state

### Key Components

#### AuthContext
- Manages user session globally
- Provides login/logout functions
- Auto-loads session from localStorage
- Exposes `getAuthHeader()` for API calls

#### VerificationForm
- Controlled textarea input
- Character count display
- Loading state handling
- Form validation

#### ResultCard
- Displays verdict badge
- Shows confidence bar with dynamic updates
- Expandable AI summary
- Community feedback integration

---

## 📝 Resume-Ready Points

### Full Project Description
> Developed a full-stack Fake News Detection web application that leverages AI-powered fact verification combining Google Fact Check API, Custom Search API, and Google Gemini AI with a hybrid confidence scoring system that integrates community feedback for enhanced accuracy.

### Technical Bullet Points

#### Backend Development
- Architected and implemented **RESTful API** using **Spring Boot 3.3** with **Java 21**, following clean architecture patterns with Controller-Service-Repository layers
- Designed and implemented **JWT-based authentication system** with **Spring Security 6**, featuring BCrypt password encryption and stateless session management
- Integrated **three external APIs** (Google Fact Check, Custom Search, Gemini AI) using **Spring WebFlux** reactive WebClient for non-blocking HTTP calls
- Developed **hybrid confidence scoring algorithm** combining 70% AI analysis with 30% community feedback for dynamic real-time score updates
- Implemented **intelligent caching** mechanism to avoid redundant API calls for previously verified claims
- Designed **PostgreSQL database schema** with JPA/Hibernate for efficient data persistence with proper entity relationships
- Built custom **prompt engineering** solution for Gemini AI integration with structured JSON responses and graceful error handling

#### Frontend Development
- Built modern **Next.js 16** frontend with **React 19** utilizing Server-Side Rendering and App Router for optimal performance
- Implemented **global state management** using React Context API for authentication flow with localStorage persistence
- Designed **responsive UI** with **Tailwind CSS 4** supporting dark/light modes and mobile-first approach
- Created reusable component library including dynamic confidence bars, verdict badges, and toast notification system
- Developed comprehensive **API service layer** with automatic JWT token management and error handling

#### Integration & DevOps
- Configured **CORS** for secure cross-origin requests between frontend (port 3000) and backend (port 8080)
- Implemented **environment-based configuration** using Spring's externalized configuration for API keys and secrets
- Structured project with proper **gitignore** configurations for both Node.js and Java/Maven ecosystems

### Impact Statements
- Engineered verification system processing claims in under 15 seconds with timeout handling
- Designed community feedback system influencing 30% of final confidence scores
- Built secure authentication protecting user data with industry-standard encryption

### Skills Demonstrated
```
Languages:        Java 21, JavaScript (ES6+)
Backend:          Spring Boot, Spring Security, Spring Data JPA, Spring WebFlux
Frontend:         React 19, Next.js 16, Tailwind CSS
Database:         PostgreSQL, JPA/Hibernate
APIs:             REST, Google Cloud APIs, Gemini AI
Security:         JWT Authentication, BCrypt, CORS
Architecture:     MVC, Clean Architecture, Layered Architecture
Tools:            Maven, npm, Git, VS Code, IntelliJ IDEA
```

---

## 🧩 Challenges & Solutions

### Challenge 1: API Response Inconsistency
**Problem**: Gemini AI sometimes returns malformed JSON
**Solution**: Implemented robust parsing with regex extraction, fallback defaults, and graceful degradation

### Challenge 2: Real-time Confidence Updates
**Problem**: Confidence score needs to reflect both AI and community input dynamically
**Solution**: Designed hybrid calculation executed on each result fetch, not stored statically

### Challenge 3: Preventing Duplicate Verifications
**Problem**: Same claim submitted multiple times wastes API calls
**Solution**: Implemented content+author lookup before verification, returns cached result if exists

### Challenge 4: Token Management Across Tabs
**Problem**: JWT token needs to persist across browser sessions
**Solution**: Used localStorage with automatic header injection in API service layer

### Challenge 5: API Timeout Handling
**Problem**: External APIs may be slow or unresponsive
**Solution**: 15-second timeout with fallback "PENDING" verdict and graceful error messages

---

## 🚀 Future Enhancements

### Planned Features
1. **Source Citation Display** - Show fact-check sources and search results to users
2. **Verification Categories** - Politics, Health, Science, Entertainment
3. **User Reputation System** - Weight feedback based on user accuracy history
4. **Social Sharing** - Share verification results on social media
5. **Browser Extension** - One-click verification from any webpage
6. **Admin Dashboard** - Moderation tools and analytics
7. **Multi-language Support** - Internationalization for global reach
8. **Rate Limiting** - Prevent API abuse with throttling
9. **Email Notifications** - Updates on saved claims
10. **Export Reports** - Download verification history as PDF

### Technical Improvements
- Implement Redis caching for verified claims
- Add comprehensive unit and integration tests
- Set up CI/CD pipeline with GitHub Actions
- Containerize with Docker for easy deployment
- Add OpenAPI/Swagger documentation
- Implement WebSocket for real-time feedback updates

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| Backend Files | ~30 Java classes |
| Frontend Components | 7 React components |
| API Endpoints | 12 REST endpoints |
| Database Tables | 3 entities |
| External API Integrations | 3 (Fact Check, Search, Gemini) |
| Lines of Code (Est.) | ~3000+ |

---

## 🔗 Configuration

This project requires environment variables for API keys and database credentials. See `.env.example` files in each project folder for required configuration.

---

*Documentation generated for portfolio and resume purposes. This is a personal project demonstrating full-stack development capabilities.*
