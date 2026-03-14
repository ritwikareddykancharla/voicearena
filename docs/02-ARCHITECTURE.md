# VOICEARENA — Technical Architecture

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      FRONTEND                           │
│              (Three.js + Vanilla JS)                    │
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────────┐ │
│  │ Three.js │  │  Game    │  │  Audio Capture         │ │
│  │ 3D Scene │  │  State   │  │  (MediaRecorder API)   │ │
│  │ Renderer │  │  Manager │  │  + Audio Playback      │ │
│  └────┬─────┘  └────┬─────┘  └──────────┬────────────┘ │
│       │              │                    │              │
│       └──────────────┼────────────────────┘              │
│                      │                                   │
│              ┌───────┴────────┐                          │
│              │   WebSocket    │                          │
│              │   Client       │                          │
│              └───────┬────────┘                          │
└──────────────────────┼──────────────────────────────────┘
                       │ wss://
                       │
┌──────────────────────┼──────────────────────────────────┐
│                BACKEND (Cloud Run)                       │
│              Python FastAPI Server                       │
│                                                         │
│  ┌───────────────────┴────────────────────────────────┐ │
│  │              WebSocket Handler                     │ │
│  └───────┬────────────────────────────┬───────────────┘ │
│          │                            │                  │
│  ┌───────┴────────┐          ┌───────┴────────┐        │
│  │  Voice Command │          │  AI Fighter    │        │
│  │  Parser        │          │  Agent (ADK)   │        │
│  │  (Gemini)      │          │  "KRONOS"      │        │
│  └───────┬────────┘          └───────┬────────┘        │
│          │                            │                  │
│  ┌───────┴────────────────────────────┴───────────────┐ │
│  │           Gemini Live API Session                  │ │
│  │     (bidirectional audio streaming)                │ │
│  └────────────────────────────────────────────────────┘ │
│                                                         │
│  ┌────────────────┐  ┌─────────────────────────────┐   │
│  │   Game Logic   │  │  Firestore                  │   │
│  │   Engine       │  │  (leaderboard, match state)  │   │
│  └────────────────┘  └─────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| **Frontend** | Three.js + Vanilla JS | 3D rendering, lightweight, no build step needed |
| **Backend** | Python FastAPI | Fastest for WebSockets + async Gemini calls |
| **AI Voice** | Gemini Live API (native audio) | Real-time voice in/out, barge-in, affective dialogue |
| **Agent Framework** | Google ADK | Required by hackathon, orchestrates Kronos agent |
| **3D Models** | Three.js primitives + GLB models | Low-poly fighters as simple geometries or Blender exports |
| **Hosting** | Google Cloud Run | Required by hackathon, auto-scales, supports WebSockets |
| **Database** | Firestore | Leaderboards, match history |
| **Assets** | Google Flow + Imagen | Generate reference art, textures, UI elements |
| **Audio SFX** | Pre-loaded MP3/WAV | Punch/kick sounds, crowd, etc. |

---

## Google Cloud Services Used

1. **Cloud Run** — hosts the FastAPI backend
2. **Firestore** — stores leaderboards, player stats, match history
3. **Gemini Live API** (via Vertex AI) — real-time voice processing
4. **ADK** — agent orchestration for Kronos
5. **Cloud Storage** — serves 3D assets and sound files
6. **Imagen** (via Flow) — asset generation for characters/backgrounds

This checks the "at least one Google Cloud service" requirement multiple times over.

---

## Key API Flows

### Flow 1: Player Voice → Fighter Action

```
1. Browser captures mic audio (PCM 16kHz)
2. Audio streamed via WebSocket to FastAPI backend
3. Backend forwards audio to Gemini Live API session
4. Gemini transcribes + classifies intent:
   - Returns: { "action": "uppercut", "confidence": 0.95 }
5. Backend validates action against game state
   (e.g., can't attack while stunned)
6. Backend sends action to frontend via WebSocket:
   { "type": "player_action", "action": "uppercut" }
7. Three.js plays uppercut animation on player model
8. Game logic calculates damage if AI didn't block
9. Frontend updates HP bars
```

### Flow 2: AI Opponent Response

```
1. After player action resolves, backend triggers AI turn
2. ADK agent (Kronos) evaluates:
   - Player's last 5 moves (pattern detection)
   - Current HP differential
   - Round timer
3. Kronos decides: { "action": "block", "taunt": true }
4. If taunt=true, Gemini Live API generates voice response
   in Kronos's persona ("Nice try, but I saw that coming!")
5. Audio streamed back to frontend for playback
6. Simultaneously, Three.js plays block animation on AI model
7. Game state updates
```

### Flow 3: Barge-In (Player Interrupts AI)

```
1. Kronos is mid-taunt, audio playing in browser
2. Player shouts "UPPERCUT!" 
3. Browser detects voice activity, sends audio to backend
4. Gemini Live API handles barge-in natively:
   - Stops Kronos's current audio output
   - Processes new player command
5. Backend sends interrupt event to frontend:
   { "type": "interrupt", "action": "uppercut" }
6. Three.js: Kronos's taunt animation cancels → hit reaction
7. Damage applied (bonus damage for interrupt = reward for using barge-in)
```

---

## Gemini Live API Configuration

```python
from google import genai
from google.genai import types

client = genai.Client(vertexai=True, project="voicearena", location="us-central1")

# Kronos system prompt
KRONOS_SYSTEM = """
You are KRONOS, an AI fighting champion in a voice-controlled fighting game.
You have a deep, cocky personality. You love trash-talking your opponent.

YOUR DUAL ROLE:
1. VOICE COMMAND PARSER: When the player speaks a command (punch, kick, 
   block, dodge, etc.), respond with a JSON action:
   {"role": "action", "command": "uppercut"}
   
2. FIGHT COMMENTATOR: Between actions, you trash-talk the player.
   Keep taunts SHORT (under 10 words). Be funny, not mean.
   Examples: "Is that all you got?" / "Too slow!" / "Ooh, that tickled!"

RULES:
- If player says an attack command → parse it, don't trash-talk
- If there's a pause in combat → deliver a short taunt
- If player interrupts you → acknowledge it ("Oh you cheeky—!")
- Adapt your tone: losing = more desperate, winning = more cocky
"""

config = types.LiveConnectConfig(
    response_modalities=["AUDIO"],
    system_instruction=KRONOS_SYSTEM,
    speech_config=types.SpeechConfig(
        voice_config=types.VoiceConfig(
            prebuilt_voice_config=types.PrebuiltVoiceConfig(
                voice_name="Orus"  # deep, authoritative voice for Kronos
            )
        )
    ),
)

# Connect
session = await client.aio.live.connect(
    model="gemini-2.5-flash-native-audio",
    config=config,
)
```

---

## ADK Agent Setup

```python
from google.adk import Agent, Tool

# Game state tool - lets Kronos access fight state
@Tool
def get_game_state() -> dict:
    """Returns current game state including HP, round, and player patterns."""
    return {
        "player_hp": game.player_hp,
        "ai_hp": game.ai_hp,
        "round": game.current_round,
        "player_last_moves": game.player_history[-5:],
        "ai_last_moves": game.ai_history[-5:],
        "time_remaining": game.round_timer,
    }

@Tool
def execute_ai_move(action: str) -> dict:
    """Execute an AI fighter action. Valid: punch, hook, uppercut, kick, 
    roundhouse, block, dodge_left, dodge_right, duck, counter, taunt."""
    result = game.execute_ai_action(action)
    return result

kronos_agent = Agent(
    name="kronos",
    model="gemini-2.5-flash",
    system_instruction=KRONOS_SYSTEM,
    tools=[get_game_state, execute_ai_move],
)
```

---

## Frontend: Three.js Scene Structure

```
Scene
├── Camera (perspective, side-view angle)
├── Lights
│   ├── Ambient light (dim)
│   ├── Spot light on player (blue tint)
│   └── Spot light on AI (red tint)
├── Arena
│   ├── Floor (hexagonal platform geometry)
│   ├── Ropes/boundary (simple lines or cylinders)
│   └── Background (dark gradient + particles)
├── Player Fighter
│   ├── Body (low-poly humanoid, blue material)
│   ├── Animation mixer (idle, punch, kick, block, hit, ko)
│   └── Health bar (HTML overlay or 3D sprite)
├── AI Fighter (Kronos)
│   ├── Body (low-poly humanoid, red material, slightly bigger)
│   ├── Animation mixer (same set)
│   └── Health bar
├── UI Overlays (HTML/CSS on top of canvas)
│   ├── HP bars
│   ├── Round indicator
│   ├── Timer
│   ├── Special meter
│   ├── Voice indicator (pulsing mic icon when listening)
│   └── Captions (what Kronos is saying)
└── Particle Systems
    ├── Hit sparks
    ├── Block sparks
    └── KO explosion
```

---

## Animation System

Since we need this done in 2 days, we use **procedural animations** built with Three.js — no external model files needed.

### Fighter Model: Built from Three.js primitives
```
- Head: SphereGeometry
- Torso: BoxGeometry
- Arms: CylinderGeometry x2
- Legs: CylinderGeometry x2
- Hands: SphereGeometry x2 (fists)
```

### Animation Approach: Tween-based
Each action is a function that tweens body parts:

```javascript
function animateUppercut(fighter) {
    // Wind up (0-200ms)
    tween(fighter.rightArm.rotation, { x: -1.5 }, 200)
    tween(fighter.torso.position, { y: -0.1 }, 200)
    
    // Strike (200-400ms)  
    tween(fighter.rightArm.rotation, { x: 0.8 }, 150)
    tween(fighter.torso.position, { y: 0.2 }, 150)
    tween(fighter.rightHand.position, { y: 1.5 }, 150)
    
    // Return to idle (400-600ms)
    tween(fighter.rightArm.rotation, { x: 0 }, 200)
    tween(fighter.torso.position, { y: 0 }, 200)
}
```

This means: **NO external 3D model files needed. Fighters are built from code.**
Flow-generated images are used for UI, backgrounds, and textures only.

---

## File Structure

```
voicearena/
├── README.md                    # Setup instructions for judges
├── architecture.png             # Architecture diagram (for submission)
├── docker-compose.yml
├── Dockerfile
│
├── backend/
│   ├── main.py                  # FastAPI server + WebSocket
│   ├── game_engine.py           # Fight logic, HP, rounds, damage calc
│   ├── kronos_agent.py          # ADK agent setup
│   ├── voice_handler.py         # Gemini Live API session management
│   ├── command_parser.py        # Voice → action mapping
│   └── requirements.txt
│
├── frontend/
│   ├── index.html               # Main game page
│   ├── css/
│   │   └── style.css            # UI overlay styles
│   ├── js/
│   │   ├── main.js              # Entry point, scene setup
│   │   ├── fighter.js            # Fighter class (build from primitives)
│   │   ├── animations.js         # All tween-based fight animations
│   │   ├── game-state.js         # Frontend game state manager
│   │   ├── websocket.js          # WebSocket client
│   │   ├── audio.js              # Mic capture + AI audio playback
│   │   ├── ui.js                 # HP bars, timer, round display
│   │   └── particles.js          # Hit effects, KO effects
│   ├── assets/
│   │   ├── sounds/               # Pre-loaded SFX
│   │   ├── textures/             # Flow-generated textures
│   │   └── ui/                   # Flow-generated UI elements
│   └── lib/
│       └── three.min.js          # Three.js library
│
├── deploy/
│   ├── cloudbuild.yaml           # Automated Cloud Build (bonus points)
│   └── terraform/                # IaC for bonus points
│       └── main.tf
│
└── docs/
    ├── 01-GAME-DESIGN.md
    ├── 02-ARCHITECTURE.md
    └── 03-BUILD-GUIDE.md
```

---

## Deployment: Google Cloud Run

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY backend/requirements.txt .
RUN pip install -r requirements.txt
COPY backend/ ./backend/
COPY frontend/ ./frontend/
CMD ["uvicorn", "backend.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

```bash
# Deploy
gcloud run deploy voicearena \
  --source . \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars GOOGLE_CLOUD_PROJECT=your-project-id
```

---

## Latency Budget

| Step | Target Latency |
|---|---|
| Audio capture → backend | ~50ms (WebSocket) |
| Gemini Live API processing | ~200-500ms (native audio, no STT/TTS pipeline) |
| Backend game logic | ~10ms |
| Backend → frontend action | ~50ms |
| Three.js animation start | ~16ms (next frame) |
| **Total voice-to-action** | **~300-600ms** |

This is fast enough to feel responsive. For comparison, most fighting games have 200-300ms input delay on standard controllers.
