# Fake News Detection System - Project Documentation

> **Purpose**: Portfolio showcase and resume reference.

---

## 📋 Table of Contents

1. [Project Overview](#-project-overview)
2. [Technical Architecture](#-technical-architecture)
3. [Technology Stack](#-technology-stack)
4. [Key Features](#-key-features)
5. [Resume-Ready Points](#-resume-ready-points)
6. [Challenges & Solutions](#-challenges--solutions)
7. [Future Enhancements](#-future-enhancements)

---

## 🎯 Project Overview

### Summary
A full-stack web application that combats misinformation by providing AI-powered fact-checking with community-driven feedback. Users can submit news headlines, claims, or statements and receive intelligent verification results combining multiple AI sources with crowd-sourced validation.

### Problem Statement
In the digital age, misinformation spreads rapidly across social media and news outlets. This application addresses the critical need for accessible, reliable fact-checking tools that combine artificial intelligence with human collective intelligence.

### Solution
An intelligent verification system that:
- Aggregates data from multiple Google AI services for comprehensive analysis
- Implements a hybrid confidence scoring algorithm combining AI and community feedback
- Provides clear, actionable verdicts: TRUE, FALSE, PARTIALLY_TRUE, UNVERIFIABLE

### Target Users
- Social media users wanting to verify viral content
- Journalists and researchers fact-checking sources
- General public seeking reliable news verification

---

## 🏗 Technical Architecture

### High-Level Architecture
```
┌─────────────────────────────────────────────────────────────────────┐
│                         Frontend (Next.js)                          │
│         Dashboard │ Login/Register │ History │ Landing Page         │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ REST API (JSON)
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                     Backend (Spring Boot)                           │
│    Security Layer → Controllers → Services → Repositories           │
└──────────────────────────────┬──────────────────────────────────────┘
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
2. **Backend receives claim** → Checks for existing verification
3. **If new claim** → Calls verification pipeline with multiple AI services
4. **Results aggregated** → Verdict + confidence score generated
5. **Response returned** → Frontend displays with community feedback options
6. **Community votes** → Dynamic confidence recalculation

---

## 🛠 Technology Stack

### Backend
| Technology | Purpose |
|------------|---------|
| **Java 21** | Core programming language |
| **Spring Boot 3.3** | Application framework |
| **Spring Security 6** | Authentication & authorization |
| **Spring Data JPA** | Database ORM |
| **Spring WebFlux** | Reactive web client for API calls |
| **PostgreSQL** | Relational database |
| **JWT** | Token-based authentication |
| **Maven** | Build tool & dependency management |

### Frontend
| Technology | Purpose |
|------------|---------|
| **Next.js 16** | React framework with SSR/SSG |
| **React 19** | UI component library |
| **Tailwind CSS 4** | Utility-first CSS framework |

### External APIs
| API | Purpose |
|-----|---------|
| **Google Fact Check API** | Access existing fact-check database |
| **Google Custom Search API** | Web search for supporting evidence |
| **Google Gemini AI** | AI-powered content analysis |

---

## ✨ Key Features

### 1. AI-Powered Fact Verification
- **Multi-source verification**: Combines multiple Google AI services
- **Intelligent prompt engineering**: Structured prompts for consistent AI responses
- **Graceful degradation**: Fallback handling if any API fails

### 2. Hybrid Confidence Scoring
- AI provides initial scientific assessment (70% weight)
- Community feedback adds human validation layer (30% weight)
- Real-time score updates as users vote

### 3. User Authentication & Authorization
- JWT-based stateless authentication
- BCrypt password encryption
- Protected routes with automatic token management

### 4. Community Feedback System
- Like/dislike mechanism for verification results
- One vote per user per verification
- Real-time statistics display

### 5. Verification History
- Persistent storage for recent verifications
- Quick access to past fact-checks
- Management capabilities

### 6. Verdicts System
| Verdict | Description |
|---------|-------------|
| **TRUE** | Claim verified as accurate |
| **FALSE** | Claim determined to be false |
| **PARTIALLY_TRUE** | Claim contains some truth but is misleading |
| **UNVERIFIABLE** | Insufficient evidence to determine accuracy |

### 7. Responsive UI/UX
- Mobile-first responsive design
- Dark/light mode support
- Loading states with animations
- Toast notifications for user feedback

---

## 📝 Resume-Ready Points

### Full Project Description
> Developed a full-stack Fake News Detection web application that leverages AI-powered fact verification combining Google Fact Check API, Custom Search API, and Google Gemini AI with a hybrid confidence scoring system that integrates community feedback for enhanced accuracy.

### Technical Bullet Points

#### Backend Development
- Architected and implemented **RESTful API** using **Spring Boot 3.3** with **Java 21**, following clean architecture patterns
- Designed and implemented **JWT-based authentication system** with **Spring Security 6**, featuring BCrypt password encryption
- Integrated **three external APIs** using **Spring WebFlux** reactive WebClient for non-blocking HTTP calls
- Developed **hybrid confidence scoring algorithm** combining AI analysis with community feedback
- Implemented **intelligent caching** mechanism to avoid redundant API calls
- Designed **PostgreSQL database schema** with JPA/Hibernate for efficient data persistence
- Built custom **prompt engineering** solution for Gemini AI integration

#### Frontend Development
- Built modern **Next.js 16** frontend with **React 19** utilizing Server-Side Rendering and App Router
- Implemented **global state management** using React Context API for authentication flow
- Designed **responsive UI** with **Tailwind CSS 4** supporting dark/light modes
- Created reusable component library including dynamic confidence bars and verdict badges
- Developed comprehensive **API service layer** with automatic JWT token management

#### Integration & DevOps
- Configured **CORS** for secure cross-origin requests
- Implemented **environment-based configuration** for secure credential management
- Structured project with proper version control practices

### Impact Statements
- Engineered verification system processing claims efficiently with timeout handling
- Designed community feedback system influencing final confidence scores
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
Tools:            Maven, npm, Git
```

---

## 🧩 Challenges & Solutions

### Challenge 1: API Response Inconsistency
**Problem**: AI services sometimes return inconsistent responses
**Solution**: Implemented robust parsing with fallback defaults and graceful degradation

### Challenge 2: Real-time Confidence Updates
**Problem**: Confidence score needs to reflect both AI and community input dynamically
**Solution**: Designed hybrid calculation executed on each result fetch

### Challenge 3: Preventing Duplicate Verifications
**Problem**: Same claim submitted multiple times wastes API calls
**Solution**: Implemented lookup mechanism before verification, returns cached result if exists

### Challenge 4: Token Management
**Problem**: JWT token needs to persist across browser sessions
**Solution**: Used localStorage with automatic header injection in API service layer

### Challenge 5: API Timeout Handling
**Problem**: External APIs may be slow or unresponsive
**Solution**: Timeout handling with fallback verdict and graceful error messages

---

## 🚀 Future Enhancements

### Planned Features
- Source Citation Display
- Verification Categories (Politics, Health, Science, etc.)
- User Reputation System
- Social Sharing
- Browser Extension
- Admin Dashboard
- Multi-language Support
- Rate Limiting
- Email Notifications
- Export Reports

### Technical Improvements
- Redis caching for verified claims
- Comprehensive unit and integration tests
- CI/CD pipeline with GitHub Actions
- Docker containerization
- OpenAPI/Swagger documentation
- WebSocket for real-time updates

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| Backend Classes | ~30 |
| Frontend Components | 7 |
| API Endpoints | 12 |
| Database Entities | 3 |
| External API Integrations | 3 |

---

*This is a personal project demonstrating full-stack development capabilities.*
