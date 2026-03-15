# 🥊 VoiceArena

**Command your fighter with your voice. The world's first voice-controlled 3D fighting game.**

> Built for the [Gemini Live Agent Challenge](https://geminiliveagentchallenge.devpost.com/) | #GeminiLiveAgentChallenge

![VoiceArena Banner](Fighters_facing_in_arena_b9284bd4be.jpeg)

<br>

## 🎮 What is VoiceArena?

VoiceArena is a browser-based 3D fighting game where **your voice is the controller**. Shout "Punch!", "Uppercut!", "Dodge left!" and watch your fighter execute in real-time. Your opponent — **KRONOS** — is an AI fighter powered by Gemini Live API who fights back, trash-talks you, adapts to your style, and can be interrupted mid-sentence.

No buttons. No keyboard. Just your voice vs AI.

### Key Features

- **Voice-Controlled Combat** — 10+ distinct voice commands processed in real-time
- **AI Opponent with Personality** — Kronos trash-talks, adapts to your patterns, and changes tone based on who's winning
- **Barge-In Mechanic** — Interrupt Kronos mid-taunt for a surprise attack (bonus damage!)
- **3D Low-Poly Visuals** — Clean geometric fighters rendered in Three.js
- **Adaptive AI** — Spam the same move and Kronos learns to counter it
- **Best-of-3 Rounds** — Health bars, timers, special meter, and KO sequences

<br>

## 🏗️ Architecture

```
┌──────────────────────────────────────────────┐
│            BROWSER (Frontend)                │
│  Three.js 3D Scene + Audio Capture/Playback  │
│              WebSocket Client                │
└─────────────────────┬────────────────────────┘
                      │ wss://
┌─────────────────────┴────────────────────────┐
│         CLOUD RUN (Backend)                  │
│            Python FastAPI                    │
│                                              │
│  ┌────────────────┐  ┌────────────────────┐  │
│  │  Game Engine   │  │  Kronos Agent      │  │
│  │  (HP, rounds,  │  │  (ADK + Gemini     │  │
│  │   damage)      │  │   Live API)        │  │
│  └────────────────┘  └────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │     Gemini Live API Session            │  │
│  │  (bidirectional native audio stream)   │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
         │                        │
   ┌─────┴──────┐          ┌─────┴──────┐
   │  Firestore │          │   Cloud    │
   │ Leaderboard│          │  Storage   │
   └────────────┘          └────────────┘
```

<br>

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Three.js, Vanilla JS, HTML/CSS |
| Backend | Python, FastAPI, WebSockets |
| AI Voice | Gemini Live API (native audio) |
| Agent | Google ADK |
| Hosting | Google Cloud Run |
| Database | Google Firestore |
| Assets | Google Cloud Storage |
| Asset Gen | Google Flow / Imagen |

<br>

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- Google Cloud account with billing enabled
- Gemini API access (Vertex AI)
- `gcloud` CLI installed and authenticated

### 1. Clone the repo

```bash
git clone https://github.com/ritwikareddykancharla/voicearena.git
cd voicearena
```

### 2. Set up environment

```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Configure Google Cloud

```bash
# Authenticate
gcloud auth application-default login

# Set your project
gcloud config set project YOUR_PROJECT_ID

# Enable required APIs
gcloud services enable \
  aiplatform.googleapis.com \
  run.googleapis.com \
  firestore.googleapis.com \
  storage.googleapis.com
```

### 4. Set environment variables

```bash
export GOOGLE_CLOUD_PROJECT=your-project-id
export GOOGLE_CLOUD_LOCATION=us-central1
```

### 5. Run locally

```bash
# From the project root
uvicorn backend.main:app --reload --port 8080
```

Open `http://localhost:8080` in Chrome (mic access required).

### 6. Deploy to Cloud Run

```bash
gcloud run deploy voicearena \
  --source . \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars GOOGLE_CLOUD_PROJECT=your-project-id
```

<br>

## 🎯 How to Play

1. Open VoiceArena in your browser
2. Allow microphone access
3. Click **START FIGHT**
4. Shout commands at your fighter:

| Command | Action |
|---|---|
| "Punch" / "Jab" | Quick punch |
| "Hook" | Wide hook |
| "Uppercut" | Rising uppercut (high damage) |
| "Kick" | Mid kick |
| "Roundhouse" | Spinning kick (high damage) |
| "Block" | Raise guard |
| "Dodge left/right" | Sidestep |
| "Duck" | Crouch under high attacks |
| "Counter" | Parry + quick strike |
| "Taunt" | Taunt (builds special meter) |
| "Special" | Ultimate move (needs full meter) |

**Pro tip:** Interrupt Kronos while he's trash-talking to land a surprise hit for bonus damage!

<br>

## 🤖 Meet KRONOS

Kronos is your AI opponent. He's not just a bot — he has personality.

- **Cocky when winning:** "Is that all you got? My grandmother hits harder."
- **Desperate when losing:** "Lucky shots! You can't keep this up!"
- **Adapts to your style:** Spam punches and he'll start blocking. Go defensive and he gets aggressive.
- **Can be interrupted:** His trash-talk is powered by Gemini Live API's barge-in feature. Cut him off mid-sentence to attack!

<br>

## 📂 Project Structure

```
voicearena/
├── README.md
├── Dockerfile
├── docker-compose.yml
├── backend/
│   ├── main.py              # FastAPI server + WebSocket
│   ├── game_engine.py       # Fight logic, HP, rounds
│   ├── kronos_agent.py      # ADK agent setup
│   ├── voice_handler.py     # Gemini Live API session
│   ├── command_parser.py    # Voice → action mapping
│   └── requirements.txt
├── frontend/
│   ├── index.html
│   ├── js/
│   │   ├── main.js          # Three.js scene setup
│   │   ├── fighter.js       # Low-poly fighter class
│   │   ├── animations.js    # Combat animations
│   │   ├── game-state.js    # Frontend state manager
│   │   ├── websocket.js     # WebSocket client
│   │   ├── audio.js         # Mic capture + playback
│   │   ├── ui.js            # HP bars, timer, round UI
│   │   └── particles.js     # Hit effects
│   ├── css/style.css
│   └── assets/
│       ├── sounds/          # SFX (punch, kick, crowd)
│       ├── textures/        # Generated textures
│       └── ui/              # Generated UI elements
├── deploy/
│   └── cloudbuild.yaml      # Automated deployment
└── docs/
    ├── 01-GAME-DESIGN.md
    ├── 02-ARCHITECTURE.md
    └── 03-BUILD-GUIDE.md
```

<br>

## ☁️ Google Cloud Services

| Service | Usage |
|---|---|
| **Gemini Live API** | Real-time bidirectional voice — player commands in, Kronos voice out |
| **ADK** | Agent orchestration for Kronos (pattern detection, strategy, personality) |
| **Cloud Run** | Hosts the FastAPI backend with WebSocket support |
| **Firestore** | Stores match history, leaderboards, player stats |
| **Cloud Storage** | Serves static 3D assets and sound files |
| **Imagen (Flow)** | Generated character concepts, UI elements, textures |

<br>

## 🔑 Key Gemini Live API Features Showcased

- **Native Audio** — No STT/TTS pipeline. Raw audio in, natural voice out. Sub-second latency.
- **Barge-In** — Player can interrupt Kronos at any time. This is a core game mechanic.
- **Affective Dialogue** — Kronos's emotional tone adapts to fight context.
- **Tool Use** — Kronos uses ADK tools to query game state and execute moves.

<br>

## 📹 Demo Video

[Watch the demo on YouTube](YOUR_YOUTUBE_LINK_HERE)

<br>

## 📄 License

VoiceArena — Voice-controlled 3D fighting game  
Copyright (C) 2026 Ritwika Reddy Kancharla

Licensed under AGPL-3.0 — see [LICENSE](LICENSE) for details.

<br>

## 🙏 Acknowledgments

- Built with [Google Gemini Live API](https://ai.google.dev/gemini-api/docs/live-api) and [ADK](https://google.github.io/adk-docs/)
- 3D rendering powered by [Three.js](https://threejs.org/)
- Sound effects from [Freesound](https://freesound.org/) and [Mixkit](https://mixkit.co/)

---

*Built for the Gemini Live Agent Challenge 2026* 🏆
