# VOICEARENA — 2-Day Build Guide

## Timeline: 48 Hours to Submission

---

## DAY 1 (Hours 1-12): CORE ENGINE

### Hour 1-2: Project Setup
```bash
# Create project structure
mkdir -p voicearena/{backend,frontend/{js,css,assets/{sounds,textures,ui}},deploy,docs}
cd voicearena

# Backend setup
cd backend
python -m venv venv
source venv/bin/activate
pip install fastapi uvicorn websockets google-genai google-adk google-cloud-firestore

# Frontend — no build step, just serve static files
# Download Three.js
cd ../frontend/lib
curl -O https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js
```

### Hour 2-4: Three.js Scene + Low-Poly Fighters

Build the 3D scene first so you have something visual to test against.

**Priority order:**
1. Basic scene with lighting and camera
2. Low-poly fighter from primitives (boxes, spheres, cylinders)
3. Idle stance animation (subtle breathing/bobbing)
4. One attack animation (punch)
5. Hit reaction animation
6. Basic UI overlay (HP bars)

**Key tip:** Use `THREE.Group()` for each fighter. Each body part is a child of the group. Animate by tweening rotation/position of individual parts.

### Hour 4-6: Game State + WebSocket

Build the game state manager:
- Player HP, AI HP
- Round counter
- Move queue
- Damage calculator

Build WebSocket connection:
- Frontend connects to backend
- Backend sends game events
- Frontend renders them

**Test with keyboard first!** Map keyboard keys to the same actions voice will trigger later. This lets you test the game without voice.

```javascript
// Temporary keyboard controls for testing
document.addEventListener('keydown', (e) => {
    const keyMap = {
        'j': 'punch', 'k': 'kick', 'l': 'uppercut',
        'u': 'hook', 'i': 'roundhouse',
        'a': 'dodge_left', 'd': 'dodge_right',
        's': 'block', 'w': 'duck',
    };
    if (keyMap[e.key]) sendAction(keyMap[e.key]);
});
```

### Hour 6-8: FastAPI Backend + Game Logic

```python
# main.py - core structure
from fastapi import FastAPI, WebSocket
from fastapi.staticfiles import StaticFiles

app = FastAPI()

# Serve frontend
app.mount("/static", StaticFiles(directory="frontend"), name="static")

@app.websocket("/ws/game")
async def game_websocket(websocket: WebSocket):
    await websocket.accept()
    game = GameSession()
    
    while True:
        data = await websocket.receive_json()
        
        if data["type"] == "player_action":
            # Process player action
            result = game.process_player_action(data["action"])
            await websocket.send_json(result)
            
            # AI responds
            ai_result = game.process_ai_turn()
            await websocket.send_json(ai_result)
```

### Hour 8-10: AI Fight Logic (No Voice Yet)

Build Kronos's brain as a simple state machine first:

```python
class KronosAI:
    def __init__(self):
        self.state = "AGGRESSIVE"
        self.player_history = []
    
    def decide_action(self, game_state):
        # Track patterns
        self.player_history.append(game_state["last_player_move"])
        
        # Adapt state
        if game_state["ai_hp"] < 30:
            self.state = "DEFENSIVE"
        elif game_state["player_hp"] < game_state["ai_hp"]:
            self.state = "AGGRESSIVE"
        
        # Counter patterns
        if self._detect_spam():
            return self._counter_spam()
        
        # State-based action
        if self.state == "AGGRESSIVE":
            return random.choice(["punch", "hook", "kick", "uppercut"])
        elif self.state == "DEFENSIVE":
            return random.choice(["block", "dodge_left", "dodge_right", "counter"])
    
    def _detect_spam(self):
        """If player used same move 3+ times in last 5"""
        if len(self.player_history) < 3:
            return False
        last_5 = self.player_history[-5:]
        return any(last_5.count(m) >= 3 for m in set(last_5))
    
    def _counter_spam(self):
        """Counter the spammed move"""
        spammed = max(set(self.player_history[-5:]), 
                      key=self.player_history[-5:].count)
        counters = {
            "punch": "block", "kick": "dodge_left",
            "uppercut": "duck", "hook": "dodge_right",
        }
        return counters.get(spammed, "block")
```

### Hour 10-12: Polish Core Game Loop

By end of Day 1 you should have:
- ✅ 3D scene with two fighters
- ✅ At least 4 animations working (punch, kick, block, hit)
- ✅ Game state with HP, rounds, damage
- ✅ WebSocket communication
- ✅ AI making smart decisions
- ✅ Keyboard controls for testing
- ✅ Basic UI (HP bars, round counter)

**Commit and push to GitHub.**

---

## DAY 2 (Hours 13-24): VOICE + POLISH + SUBMIT

### Hour 13-16: Gemini Live API Integration

This is the critical part. Wire up voice.

**Browser side — capture mic audio:**
```javascript
class VoiceCapture {
    async start() {
        const stream = await navigator.mediaDevices.getUserMedia({ 
            audio: { sampleRate: 16000, channelCount: 1 } 
        });
        
        const audioContext = new AudioContext({ sampleRate: 16000 });
        const source = audioContext.createMediaStreamSource(stream);
        const processor = audioContext.createScriptProcessor(4096, 1, 1);
        
        processor.onaudioprocess = (e) => {
            const pcmData = e.inputBuffer.getChannelData(0);
            // Convert to Int16 and send via WebSocket
            const int16 = new Int16Array(pcmData.length);
            for (let i = 0; i < pcmData.length; i++) {
                int16[i] = Math.max(-32768, Math.min(32767, pcmData[i] * 32768));
            }
            this.ws.send(int16.buffer);
        };
        
        source.connect(processor);
        processor.connect(audioContext.destination);
    }
}
```

**Backend — Gemini Live API session:**
```python
async def handle_voice_session(websocket, game):
    client = genai.Client(vertexai=True, project=PROJECT, location="us-central1")
    
    config = types.LiveConnectConfig(
        response_modalities=["AUDIO", "TEXT"],
        system_instruction=KRONOS_SYSTEM_PROMPT,
        speech_config=types.SpeechConfig(
            voice_config=types.VoiceConfig(
                prebuilt_voice_config=types.PrebuiltVoiceConfig(
                    voice_name="Orus"
                )
            )
        ),
    )
    
    async with client.aio.live.connect(
        model="gemini-2.5-flash-native-audio",
        config=config,
    ) as session:
        
        # Task 1: Forward player audio to Gemini
        async def send_audio():
            while True:
                audio_data = await websocket.receive_bytes()
                await session.send(audio_data, mime_type="audio/pcm")
        
        # Task 2: Receive Gemini responses
        async def receive_responses():
            async for response in session.receive():
                if response.text:
                    # Parse command from text
                    action = parse_command(response.text)
                    if action:
                        result = game.process_player_action(action)
                        await websocket.send_json(result)
                
                if response.audio:
                    # Forward Kronos voice to browser
                    await websocket.send_bytes(response.audio.data)
        
        await asyncio.gather(send_audio(), receive_responses())
```

### Hour 16-18: ADK Agent Wiring

Connect the ADK agent properly:

```python
from google.adk import Agent

kronos = Agent(
    name="kronos_fighter",
    model="gemini-2.5-flash",
    system_instruction="""
    You are KRONOS, the AI champion in VoiceArena.
    
    When you receive game state updates, decide your next action.
    Use get_game_state to check the fight status.
    Use execute_move to perform your chosen action.
    
    After each exchange, deliver a SHORT trash-talk line (under 10 words).
    Be cocky when winning, desperate when losing.
    """,
    tools=[get_game_state_tool, execute_move_tool],
)
```

### Hour 18-20: More Animations + Visual Polish

Add remaining animations:
- Uppercut, hook, roundhouse, kick
- Dodge left/right, duck
- Combo sequence
- KO fall animation
- Special move (dramatic camera zoom + big hit)
- Hit sparks (particle system)
- Round transition screen

Add visual juice:
- Screen shake on big hits
- Slow-mo on KO
- Particle effects on impacts
- Kronos eyes glow red when aggressive
- Camera slight zoom on special moves

### Hour 20-22: Cloud Run Deployment

```bash
# Build and deploy
gcloud builds submit --tag gcr.io/PROJECT_ID/voicearena
gcloud run deploy voicearena \
    --image gcr.io/PROJECT_ID/voicearena \
    --region us-central1 \
    --allow-unauthenticated \
    --memory 512Mi \
    --timeout 300

# Set up Firestore
gcloud firestore databases create --location=us-central1
```

Test the deployed version. Make sure WebSocket works over HTTPS (wss://).

### Hour 22-23: Record Demo Video (< 4 minutes)

**Demo video structure:**

```
0:00 - 0:20  HOOK
  "What if you could fight an AI... with just your voice?"
  Show title screen, dramatic music

0:20 - 0:40  THE PROBLEM
  "Fighting games need controllers. Voice AI is stuck in text boxes.
   VoiceArena breaks both paradigms."

0:40 - 2:30  LIVE GAMEPLAY DEMO
  - Start a match against Kronos
  - Show voice commands working: "Punch!" "Dodge!" "Uppercut!"
  - Show Kronos trash-talking back
  - Show barge-in: interrupt Kronos mid-taunt with a surprise attack
  - Show AI adapting: spam punches, then Kronos starts blocking
  - Win the round (or lose dramatically — both are entertaining)

2:30 - 3:15  TECH DEEP DIVE
  - Show architecture diagram
  - Highlight: Gemini Live API, ADK, Cloud Run, Firestore
  - Show Google Cloud Console proving it's running on GCP
  - Mention key features: barge-in, affective dialogue, adaptive AI

3:15 - 3:45  WRAP UP
  "VoiceArena proves AI interaction doesn't need a text box.
   It needs a fighting ring."
  Show logo, team info
```

### Hour 23-24: Devpost Submission

Fill in ALL fields:
- ✅ Text description (copy from game design doc)
- ✅ Public GitHub repo URL (with README + setup instructions)
- ✅ Proof of Cloud deployment (screen recording of Cloud Run console)
- ✅ Architecture diagram (clean version of the one in this doc)
- ✅ Demo video (< 4 minutes, uploaded to YouTube)

**BONUS POINTS:**
- ✅ Blog post about how you built it → publish on Dev.to or Medium
- ✅ Use hashtag #GeminiLiveAgentChallenge on X when sharing
- ✅ Automated deployment (include cloudbuild.yaml in repo)
- ✅ Join a Google Developer Group + link your profile

---

## Emergency Fallbacks

If you're running out of time:

| Problem | Quick Fix |
|---|---|
| Three.js animations too complex | Use simple position/rotation tweens, skip smooth blending |
| Gemini Live API voice not working | Fall back to regular Gemini API with browser SpeechRecognition for input |
| 3D fighters look bad | Replace with 2D sprites/images generated in Flow — still works in Three.js as planes |
| Cloud Run WebSocket issues | Deploy as a regular HTTP API with polling instead of WebSocket |
| ADK integration failing | Use Gemini API directly with function calling — same concept, simpler setup |
| No time for Firestore | Skip leaderboard, it's a nice-to-have |

---

## What Makes This Win

1. **FUN FACTOR** — Judges will want to play it. That's rare in hackathons.
2. **BARGE-IN SHOWCASE** — Interrupting the AI mid-taunt is the perfect demo of Gemini Live's killer feature.
3. **AFFECTIVE DIALOGUE** — Kronos adapts his tone to the fight. Cocky when winning, desperate when losing.
4. **ADAPTIVE AI** — Pattern detection makes it feel intelligent, not scripted.
5. **CLEAN ARCHITECTURE** — Clear separation of concerns, well-documented.
6. **VISUAL IMPACT** — 3D game in a browser, voice-controlled. Nobody else is doing this.
