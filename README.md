# AI Mock Interview Platform

Lightweight mock interview platform with a React client and Node.js/Express server.

## Repository structure

- `client/` — React front-end (Vite)
- `server/` — Express backend and services

## Features

- Conduct mock interviews with voice and code prompts
- Save interview history and feedback
- Integrations for speech and AI services

## Prerequisites

- Node.js (16+)
- npm or yarn

## Environment

Create a `.env` file in `server/` with the needed variables. Example keys:

- `MONGO_URI` — MongoDB connection string
- `JWT_SECRET` — JSON Web Token secret
- `ASSEMBLYAI_API_KEY` — AssemblyAI API key (if used)
- `MURF_API_KEY` — Murf API key (if used)
- `GEMINI_API_KEY` — Gemini API key (if used)

Do NOT commit real secrets to the repository.

## Setup

Install server and client dependencies:

```bash
cd server
npm install

cd ../client
npm install
```

## Running (development)

Start the server (development):

```bash
cd server
npm run dev
```

Start the client (development):

```bash
cd client
npm run dev
```

If `dev` scripts are not present, use `npm start` instead.

## Build

Build the client for production:

```bash
cd client
npm run build
```

## Contributing

Feel free to open issues or pull requests. For local development, run both client and server concurrently.

## License

Specify the project license here.

## Functionality Overview

We built a complete AI-powered mock interview platform across 3 functionalities and 11 steps:

**Functionality 1: Resume Upload & Interview Setup (Steps 1–4)**
1. Set up Gemini AI client and prompt templates
2. Built Resume model, PDF parsing service, and upload controller
3. Built Interview model and start interview service
4. Built `InterviewSetupPage` with 3-step wizard and initial `HomePage`

**Functionality 2: AI Interview (Steps 5–8)**
5. Built voice-to-text transcription with AssemblyAI
6. Built text-to-speech with Murf AI (streaming and non-streaming)
7. Built answer submission, code evaluation, feedback generation, and all interview routes
8. Built `InterviewPage` with `VoiceRecorder`, `AudioPlayer`, `CodeEditor`, and interviewer state machine

**Functionality 3: Feedback & Interview History (Steps 9–11)**
9. Detailed the feedback generation logic (`endInterview`, `getInterviewById`)
10. Built history service, controller, and routes with pagination
11. Built `FeedbackPage`, `HistoryPage`, complete `HomePage` with stats, and final `App.jsx` routing

## Architecture Overview

Frontend (React) connects to the Express backend via HTTP REST API. External AI services (Gemini, Murf AI, AssemblyAI) are used by backend services. MongoDB Atlas stores users, interviews, and resumes.

```
┌─────────────────────────────────────────────────────────┐
│                    React Frontend                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐ │
│  │  Setup   │→ │Interview │→ │ Feedback │  │ History │ │
│  │  Page    │  │  Page    │  │  Page    │  │  Page   │ │
│  └──────────┘  └──────────┘  └──────────┘  └─────────┘ │
│       ↓              ↓             ↓            ↓       │
│  ┌──────────────────────────────────────────────────┐   │
│  │            Axios API Service Layer               │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────┬───────────────────────────────┘
						  │ HTTP REST API
┌─────────────────────────┴───────────────────────────────┐
│                   Express Backend                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐ │
│  │  Routes  │→ │Controllers│→│ Services │→ │ Models  │ │
│  └──────────┘  └──────────┘  └──────────┘  └─────────┘ │
│                      ↓                                   │
│  ┌──────────────────────────────────────────────────┐   │
│  │         External AI Services                      │   │
│  │  ┌─────────┐  ┌──────────┐  ┌───────────────┐   │   │
│  │  │ Gemini  │  │ Murf AI  │  │  AssemblyAI   │   │   │
│  │  │  (LLM)  │  │  (TTS)   │  │   (STT)       │   │   │
│  │  └─────────┘  └──────────┘  └───────────────┘   │   │
│  └──────────────────────────────────────────────────┘   │
│                      ↓                                   │
│  ┌──────────────────────────────────────────────────┐   │
│  │              MongoDB Atlas                        │   │
│  │  ┌──────┐  ┌───────────┐  ┌──────────┐          │   │
│  │  │Users │  │Interviews │  │ Resumes  │          │   │
│  │  └──────┘  └───────────┘  └──────────┘          │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

