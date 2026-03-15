# VOICEARENA — Devpost Submission

---

## Project Name
VoiceArena

## Tagline
Command your fighter with your voice. The world's first voice-controlled 3D fighting game.

## Category
Live Agents

---

## Inspiration

Fighting games have been button-mashing experiences for 30 years. Voice AI has been trapped in text boxes. We asked: what if we combined them? What if you could shout "UPPERCUT!" and watch a 3D fighter execute it in real-time, while an AI opponent trash-talks you back?

VoiceArena was born from the realization that Gemini's Live API — with its barge-in capability, native audio, and affective dialogue — is perfect for something nobody has tried: real-time voice-controlled combat.

## What it does

VoiceArena is a browser-based 3D fighting game where your voice is the controller. You speak attack commands ("Punch! Kick! Uppercut!"), defensive moves ("Block! Dodge left!"), and even taunts — all processed in real-time by Gemini's Live API.

Your opponent is **KRONOS**, an AI fighter with a cocky personality who trash-talks you during the fight. Kronos adapts to your fighting style — if you spam the same move, he learns to counter it. And thanks to Gemini's barge-in feature, you can interrupt Kronos mid-taunt to land a surprise attack for bonus damage.

**Key features:**
- Voice-controlled combat with 10+ distinct commands
- AI opponent with adaptive fight logic and personality
- Barge-in mechanic: interrupt the AI for bonus damage
- Affective dialogue: Kronos's tone changes based on who's winning
- 3D low-poly visuals rendered in Three.js
- Best-of-3 round system with health bars, timers, and special moves

## How we built it

**Frontend:** Three.js for 3D rendering with procedural low-poly fighters built from geometric primitives. Tween-based animations for all combat moves. HTML/CSS overlay for game UI.

**Backend:** Python FastAPI with WebSocket for real-time communication. Game engine handles HP, damage calculation, round management, and AI decision-making.

**AI Voice:** Gemini Live API (native audio model) handles all voice processing. A single bidirectional audio stream captures the player's commands and streams Kronos's voice responses. The system prompt gives Kronos dual roles: command parser and trash-talking commentator.

**Agent Architecture:** Google ADK orchestrates Kronos as an autonomous agent with tools to query game state and execute moves. The agent adapts its strategy using pattern detection on the player's last 5 moves.

**Hosting:** Deployed on Google Cloud Run with Firestore for match history and leaderboards. Assets served from Cloud Storage.

**Visual Assets:** Character concepts and UI elements generated using Google Flow and Imagen, staying within the Google ecosystem.

## Google Cloud Services Used
- **Gemini Live API** (Vertex AI) — real-time voice I/O
- **ADK** — agent orchestration for Kronos
- **Cloud Run** — backend hosting
- **Firestore** — leaderboard and match data
- **Cloud Storage** — static asset serving
- **Imagen** (via Flow) — asset generation

## Challenges we ran into

1. **Voice-to-action latency:** Getting voice commands to feel responsive enough for a fighting game. We optimized by using Gemini's native audio model (no separate STT/TTS pipeline) and keeping the command vocabulary small and distinct.

2. **Barge-in timing:** Making interrupts feel fair. We added bonus damage for successful interrupts to incentivize using this Gemini Live API feature.

3. **AI personality consistency:** Keeping Kronos in character while also parsing commands accurately. Solved with a dual-role system prompt that clearly separates the parsing and commentary functions.

4. **Procedural animations:** Creating convincing fight animations without external 3D models. We built fighters from Three.js primitives and used tween-based animation functions.

## Accomplishments we're proud of

- The barge-in mechanic is genuinely fun and a perfect showcase of Gemini Live API's unique capability
- Kronos feels like a real opponent with personality, not a chatbot
- The game runs entirely in a browser with no install required
- Voice-to-action latency under 600ms feels surprisingly responsive

## What we learned

- Gemini's native audio model dramatically reduces latency compared to STT→LLM→TTS pipelines
- Affective dialogue makes AI feel alive — Kronos getting desperate when losing is a powerful moment
- Simple procedural 3D graphics can look great with good lighting and particle effects
- Game design for voice input requires a small, distinct command vocabulary

## What's next for VoiceArena

- Multiplayer: two humans voice-fighting each other with Kronos as commentator
- More fighters with different fighting styles and personalities
- Mobile support with touch + voice hybrid controls
- Tournament mode with brackets and spectators
- Integration with webcam for gesture-based special moves

## Built with
Python, FastAPI, Three.js, JavaScript, Google Gemini Live API, Google ADK, Google Cloud Run, Google Firestore, Google Cloud Storage, Google Imagen, WebSockets
