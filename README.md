# 🔍 Fake News Detection System
## 📖 Overview

A full-stack web application that combats misinformation by providing **AI-powered fact-checking** combined with **community-driven validation**. Users can submit news headlines, claims, or statements and receive intelligent verification results from multiple AI sources with crowd-sourced feedback.

### The Problem

In the digital age, misinformation spreads rapidly across social media and news outlets, making it increasingly difficult to distinguish fact from fiction.

### The Solution

An intelligent verification system that:
- 🤖 Aggregates data from multiple Google AI services for comprehensive analysis
- 📊 Implements a hybrid confidence scoring algorithm (AI + Community feedback)
- ✅ Provides clear, actionable verdicts: **TRUE**, **FALSE**, **PARTIALLY_TRUE**, **UNVERIFIABLE**

---

## ✨ Features

### 🧠 AI-Powered Verification
- **Multi-source Analysis**: Combines Google Fact Check API, Custom Search API, and Gemini AI
- **Intelligent Prompt Engineering**: Structured prompts for consistent AI responses
- **Graceful Degradation**: Fallback handling when APIs fail

### 📈 Hybrid Confidence Scoring
- AI provides initial assessment (70% weight)
- Community feedback adds human validation (30% weight)
- Real-time score updates as users vote

### 🔐 Secure Authentication
- JWT-based stateless authentication
- BCrypt password encryption
- Protected routes with automatic token management

### 👥 Community Feedback
- Like/dislike mechanism for verification results
- One vote per user per verification
- Real-time statistics display

### 🎨 Modern UI/UX
- Mobile-first responsive design
- Dark/light mode support
- Loading animations and toast notifications
- Reusable component library

---

## 🎬 Demo

### Verdict Types
| Verdict | Description |
|---------|-------------|
| ✅ **TRUE** | Claim verified as accurate |
| ❌ **FALSE** | Claim determined to be false |
| ⚠️ **PARTIALLY_TRUE** | Contains some truth but is misleading |
| ❓ **UNVERIFIABLE** | Insufficient evidence to determine accuracy |

---

## 🛠 Tech Stack

### Backend
| Technology | Purpose |
|------------|---------|
| Java 21 | Core programming language |
| Spring Boot 3.3 | Application framework |
| Spring Security 6 | Authentication & authorization |
| Spring Data JPA | Database ORM |
| Spring WebFlux | Reactive HTTP client |
| PostgreSQL | Relational database |
| JWT (jjwt 0.12.5) | Token-based auth |
| Maven | Build tool |

### Frontend
| Technology | Purpose |
|------------|---------|
| Next.js 16 | React framework with App Router |
| React 19 | UI component library |
| Tailwind CSS 4 | Utility-first styling |

### External APIs
| API | Purpose |
|-----|---------|
| Google Fact Check API | Access fact-check database |
| Google Custom Search API | Web search for evidence |
| Google Gemini AI | AI-powered content analysis |

---

## 🚀 Installation

### Prerequisites

- **Java 21** or higher
- **Node.js 18+** and npm
- **PostgreSQL 14+**
- **Maven 3.8+**
- Google Cloud API keys (Fact Check, Custom Search, Gemini)

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/fake-news-detection.git
cd fake-news-detection
```

### 2. Database Setup

```sql
-- Create PostgreSQL database
CREATE DATABASE FND;
```

### 3. Backend Setup

```bash
cd FakeNewsDetection

# Create .env file with your credentials
cp .env.example .env
```

Configure your `.env` file:
```env
DB_URL=jdbc:postgresql://localhost:5432/FND
DB_USERNAME=postgres
DB_PASSWORD=your_password

GOOGLE_FACTCHECK_KEY=your_factcheck_api_key
GOOGLE_SEARCH_KEY=your_search_api_key
GOOGLE_SEARCH_CX=your_search_engine_id
GEMINI_KEY=your_gemini_api_key

JWT_SECRET=your_base64_encoded_secret_key
```

Run the backend:
```bash
# Using Maven Wrapper
./mvnw spring-boot:run

# Or using Maven
mvn spring-boot:run
```

The backend will start on `http://localhost:8080`

### 4. Frontend Setup

```bash
cd fake-news-detector

# Install dependencies
npm install

# Run development server
npm run dev
```

The frontend will start on `http://localhost:3000`

---

## 🏗 Architecture

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

### Project Structure

```
F-News/
├── FakeNewsDetection/              # Spring Boot Backend
│   ├── src/main/java/com/FNDBackend/FakeNewsDetection/
│   │   ├── config/                 # Security configuration
│   │   ├── controller/             # REST endpoints
│   │   ├── dto/                    # Data transfer objects
│   │   ├── JwtSecurity/            # JWT authentication
│   │   ├── mapper/                 # Entity-DTO converters
│   │   ├── model/                  # JPA entities
│   │   ├── repository/             # Data access layer
│   │   └── service/                # Business logic
│   └── src/main/resources/
│       └── application.properties  # Configuration
│
├── fake-news-detector/             # Next.js Frontend
│   └── src/
│       ├── app/                    # Next.js App Router pages
│       │   ├── dashboard/          # Main verification page
│       │   ├── history/            # Past verifications
│       │   ├── login/              # Authentication
│       │   └── register/           # User registration
│       ├── components/             # Reusable UI components
│       ├── context/                # React Context (Auth)
│       └── lib/                    # API service layer
│
└── README.md
```

---

## 📡 API Reference

### Base URL
```
http://localhost:8080/api/v1
```

### Authentication Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/users/register` | Register new user |
| `POST` | `/users/login` | User login |
| `GET` | `/users/{id}` | Get user profile |

### Verification Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/messages/verify` | Submit claim for verification |
| `GET` | `/messages/history` | Get user's verification history |
| `GET` | `/messages/{id}` | Get specific verification |

### Feedback Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/feedback` | Submit like/dislike |
| `GET` | `/feedback/stats/{messageId}` | Get feedback statistics |

### Sample Request

```bash
# Verify a claim
curl -X POST http://localhost:8080/api/v1/messages/verify \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{"content": "The Earth is flat"}'
```

### Sample Response

```json
{
  "id": 1,
  "content": "The Earth is flat",
  "verdict": "FALSE",
  "confidenceScore": 95.5,
  "explanation": "Scientific evidence conclusively demonstrates that Earth is an oblate spheroid...",
  "sources": ["NASA", "National Geographic", "Scientific American"],
  "createdAt": "2026-02-18T10:30:00Z"
}
```

---

## 🧪 Running Tests

### Backend Tests
```bash
cd FakeNewsDetection
mvn test
```

### Frontend Lint
```bash
cd fake-news-detector
npm run lint
```

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| Backend Classes | ~30 |
| Frontend Components | 7 |
| API Endpoints | 12 |
| Database Entities | 3 |
| External APIs | 3 |

---

## 🗺 Roadmap

- [ ] Source citation display with links
- [ ] Verification categories (Politics, Health, Science)
- [ ] User reputation system
- [ ] Social media sharing
- [ ] Browser extension
- [ ] Admin dashboard
- [ ] Multi-language support
- [ ] Redis caching
- [ ] Docker containerization
- [ ] CI/CD with GitHub Actions
- [ ] OpenAPI/Swagger documentation

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Google Cloud Platform](https://cloud.google.com/) for AI APIs
- [Spring Boot](https://spring.io/projects/spring-boot) for the robust backend framework
- [Next.js](https://nextjs.org/) for the powerful React framework
- [Tailwind CSS](https://tailwindcss.com/) for the utility-first CSS framework

---

<div align="center">

**Built with ❤️ for fighting misinformation**

</div>
