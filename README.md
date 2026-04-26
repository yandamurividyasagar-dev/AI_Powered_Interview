# AI Mock Interview Platform — AI-Powered Mock Interview Platform
Practice like it's real. Get feedback like it matters. Resume-based questions, voice interviews, live coding, and instant AI feedback. All in one place.

## 🚀 Live Demo
➜ **Try it live → [ai-mock-interview.onrender.com](https://ai-mock-interview.onrender.com)**

No installation needed. Open the link, sign in with Google, and start your interview instantly.

## Demo Video

https://github.com/user-attachments/assets/825ab4fb-6388-4225-b293-51d31f646d99

---

## The Problem That Started This

I was preparing for technical interviews and realized something frustrating — I *knew* the answers, but I kept freezing when it came to actually saying them out loud. Watching mock interview videos wasn't enough. Reading Q&A lists wasn't enough.

> **Watching Virat Kohli bat on TV won't make you hit a six. You need to face the ball yourself.**

The gap between *knowing* and *performing* is real — and it only closes with practice under realistic conditions. So I built an AI interviewer available 24/7 that listens, evaluates your code, and gives instant detailed feedback. No scheduling constraints. No high costs.

---

## What You Can Do With It

- **Resume-Based Questions** — AI analyzes your resume and generates personalized interview questions tailored to your actual experience and projects.
- **Voice Interviews** — Listen to questions via AI voice (Murf TTS), record verbal answers, and get them transcribed automatically via AssemblyAI.
- **Live Coding** — Built-in Monaco code editor for coding challenges, with AI evaluation of your solution.
- **AI Scoring** — Detailed feedback with scores across 5 performance categories after every interview.
- **Interview History** — Every session is saved automatically so you can track your progress over time.
- **Multi-Role Support** — Frontend, Backend, Full Stack, Data Analyst, DevOps, and more.
- **Google Sign-In** — One click and you're in. No separate account needed.
- **Dashboard Stats** — See total interviews, completed count, and average score at a glance.

---

## Tech Stack

### Frontend

| Technology | Purpose |
|---|---|
| React 19 + Vite | UI framework with lightning-fast dev server |
| React Router v7 | Page navigation without full reloads |
| Monaco Editor | VS Code-grade editor with syntax highlighting |
| Axios | HTTP client with automatic JWT attachment |
| @react-oauth/google | Google Sign-In integration |
| React Hot Toast | Clean, unobtrusive user notifications |

### Backend

| Technology | Purpose |
|---|---|
| Node.js + Express 5 | REST API server |
| MongoDB + Mongoose | Database for users, resumes, and interview history |
| Google Gemini AI | Powers question generation, follow-ups, code evaluation, and feedback |
| Murf AI (TTS) | Converts AI interviewer responses into natural-sounding speech |
| AssemblyAI (STT) | Transcribes candidate voice answers to text |
| JSON Web Tokens | Secure, stateless authentication |
| bcryptjs | Password hashing |
| Google Auth Library | Server-side Google token verification |
| pdfjs-dist | PDF parsing for resume text extraction |

---

## Project Structure

```
AI-Mock-Interview/
├── client/                        # React frontend
│   └── src/
│       ├── components/            # AudioPlayer, VoiceRecorder, CodeEditor, Navbar, ScoreCard
│       ├── context/               # AuthContext — global login state
│       ├── pages/                 # Login, Home, Setup, Interview, Feedback, History
│       ├── services/              # API functions (auth, interview, history)
│       ├── constants/             # Role list, difficulty config, starter code
│       └── styles/                # CSS files per component
│
└── server/                        # Express backend
    └── src/
        ├── config/                # Gemini AI, Google OAuth, MongoDB connection
        ├── constants/             # Prompt templates for all AI operations
        ├── controllers/           # HTTP handlers for auth, resume, interview, history
        ├── middleware/            # JWT auth guard, file upload (multer), error handler
        ├── models/                # User, Resume, Interview database schemas
        ├── routes/                # API route definitions
        ├── services/              # Core logic: resume, interview, Gemini, Murf, AssemblyAI
        └── utils/                 # JWT helpers, Gemini response cleaners
```

---

## Running It Locally

### Prerequisites
- Node.js v18+
- MongoDB Atlas account (free tier is fine)
- Gemini API key → [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
- Google OAuth Client ID → [console.cloud.google.com](https://console.cloud.google.com)
- Murf AI API key → [murf.ai](https://murf.ai)
- AssemblyAI API key → [assemblyai.com](https://assemblyai.com)

### 1. Clone the repo
```bash
git clone https://github.com/your-username/AI-Mock-Interview.git
cd AI-Mock-Interview
```

### 2. Backend setup
```bash
cd server
npm install
```

Create a `.env` file in the `server/` folder:

```env
PORT=5000
CLIENT_URL=http://localhost:5173
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_secret_key_here
JWT_EXPIRES_IN=7d
GEMINI_API_KEY=your_gemini_api_key
GOOGLE_CLIENT_ID=your_google_client_id
MURF_API_KEY=your_murf_api_key
ASSEMBLYAI_API_KEY=your_assemblyai_api_key
```

```bash
npm run dev
```

If everything is connected, you'll see:
```
MongoDB Connected: cluster0.xxxxx.mongodb.net
Server running on port 5000
```

### 3. Frontend setup
Open a new terminal:

```bash
cd client
npm install
```

Create a `.env` file in the `client/` folder:

```env
VITE_API_URL=http://localhost:5000/api
VITE_GOOGLE_CLIENT_ID=your_google_client_id
```

```bash
npm run dev
```

Visit `http://localhost:5173` and you're good to go.

---

## API Endpoints

All `/interview` and `/history` routes require `Authorization: Bearer <token>` in the request header.

### Authentication

| Method | Endpoint | What It Does |
|---|---|---|
| POST | `/api/auth/register` | Create account with name, email, password |
| POST | `/api/auth/login` | Login with email and password |
| POST | `/api/auth/google` | Login with Google credential |
| GET | `/api/auth/me` | Get current logged-in user profile |
| POST | `/api/auth/logout` | Logout |

### Resume

| Method | Endpoint | What It Does |
|---|---|---|
| POST | `/api/resume/upload` | Upload PDF resume and extract text |
| GET | `/api/resume` | Retrieve saved resume for current user |

### Interview Operations

| Method | Endpoint | Request Body |
|---|---|---|
| POST | `/api/interview/start` | `{ role, resumeText, totalQuestions }` |
| POST | `/api/interview/transcribe` | multipart audio — transcription preview only |
| POST | `/api/interview/speak` | `{ text }` — streams TTS audio |
| POST | `/api/interview/:id/answer` | `{ answer }` — submit a text answer |
| POST | `/api/interview/:id/voice` | multipart audio — transcribed then submitted |
| POST | `/api/interview/:id/code` | `{ code, language }` — submit coding answer |
| POST | `/api/interview/:id/end` | Generate and save AI feedback report |
| GET | `/api/interview/:id` | Fetch a specific interview session |

### History

| Method | Endpoint | What It Does |
|---|---|---|
| GET | `/api/history?page=1&limit=8` | Fetch paginated interview history |
| GET | `/api/history/:id` | Fetch a specific history item with full details |
| DELETE | `/api/history/:id` | Delete one interview entry |
| DELETE | `/api/history/clear` | Delete all history |

---

## How an Interview Actually Works

```
User uploads resume and picks role + difficulty
        ↓
Gemini AI reads resume and generates personalized questions
        ↓
Murf TTS converts the AI interviewer's greeting to speech
        ↓
AudioPlayer plays the question out loud in the browser
        ↓
User records a voice answer via VoiceRecorder
        ↓
AssemblyAI transcribes the audio to text
        ↓
Gemini generates a contextual follow-up using full conversation history
        ↓
For coding questions, Monaco editor opens and AI evaluates the submission
        ↓
After all questions, AI generates a detailed feedback report
        ↓
Full session saved to history for progress tracking
```

### How Auth Works

```
User logs in (email/password or Google OAuth)
        ↓
Backend verifies credentials or Google token
        ↓
JWT token issued (7 day expiry)
        ↓
Token stored in localStorage
        ↓
Axios auto-attaches token to every request
        ↓
Auth middleware validates token on protected routes
        ↓
User object attached to request for downstream use
```

---

## AI Feedback & Scoring

After each interview, Gemini generates a detailed performance report scored across **5 categories**:

| Category | What's Evaluated |
|---|---|
| Technical Knowledge | Accuracy and depth of domain-specific answers |
| Communication | Clarity, structure, and conciseness of responses |
| Problem Solving | Approach, reasoning, and creativity in tackling problems |
| Code Quality | Correctness, readability, and efficiency of code submissions |
| Confidence | Tone, fluency, and composure throughout the interview |

Each category is scored out of 10, with an overall score, highlighted strengths, areas for improvement, and a final summary assessment.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                      React Frontend                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────┐  │
│  │  Setup   │→ │Interview │→ │ Feedback │  │  History  │  │
│  │  Page    │  │  Page    │  │  Page    │  │  Page     │  │
│  └──────────┘  └──────────┘  └──────────┘  └───────────┘  │
│        ↓              ↓             ↓              ↓         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              Axios API Service Layer                  │   │
│  └──────────────────────────────────────────────────────┘   │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTP + JWT
┌──────────────────────────▼──────────────────────────────────┐
│                    Express Backend                           │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────┐  │
│  │   Auth   │  │  Resume  │  │Interview │  │  History  │  │
│  │  Routes  │  │  Routes  │  │  Routes  │  │  Routes   │  │
│  └──────────┘  └──────────┘  └──────────┘  └───────────┘  │
│        ↓              ↓             ↓              ↓         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                  Service Layer                        │   │
│  └──────────────────────────────────────────────────────┘   │
│        ↓              ↓             ↓                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                  │
│  │  Gemini  │  │  Murf AI │  │AssemblyAI│                  │
│  │    AI    │  │  (TTS)   │  │  (STT)   │                  │
│  └──────────┘  └──────────┘  └──────────┘                  │
│                      ↓                                       │
│              ┌───────────────┐                              │
│              │    MongoDB    │                              │
│              └───────────────┘                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Things I Want to Add Next

- Support for JavaScript, TypeScript, Go, and Rust code challenges
- Side-by-side diff view when reviewing code evaluation feedback
- Shareable interview sessions via public links
- VS Code extension for in-editor interview practice
- Dark / light theme toggle
- Leaderboard and streak tracking for consistent practice
- Video recording mode for body language feedback
- Company-specific question packs (FAANG, startups, etc.)

---

## Author

Built by **Vidya Sagar Yandamuri**

If this project helped you or impressed you, drop a ⭐ — it genuinely motivates me to keep building.

[GitHub](https://github.com/your-username/AI-Mock-Interview) • [Live Demo](https://ai-mock-interview.onrender.com)

> *"I didn't build this to show I can use AI. I built it to understand what happens when AI becomes the interviewer — and whether that can actually make you better."*
