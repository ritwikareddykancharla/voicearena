# VOICEARENA — Game Design Document

## The Pitch (30 seconds)

VoiceArena is a real-time voice-controlled 3D fighting game where you command your fighter by **speaking**. Instead of mashing buttons, you shout "Uppercut! Dodge left! Block!" and your fighter executes in real-time. Your AI opponent — powered by Gemini Live API — fights back, talks trash, adapts to your style, and can be interrupted mid-sentence. It's the first voice-controlled fighting game that feels alive.

---

## Why This Wins

| Judging Criteria | How VoiceArena Nails It |
|---|---|
| **Innovation & Multimodal UX (40%)** | Voice-controlled combat is a completely new paradigm. No text box. The AI opponent has a distinct personality, trash-talks, and responds to your emotions via affective dialogue. Barge-in = you can interrupt the AI mid-taunt to attack. |
| **Technical Implementation (30%)** | Gemini Live API for real-time voice I/O, ADK for agent orchestration, Three.js for 3D rendering, Cloud Run for hosting, Firestore for leaderboards. Clean architecture. |
| **Demo & Presentation (30%)** | The demo IS the game. You literally play it live on camera. Nothing is more engaging than watching someone shout "UPPERCUT!" and seeing a 3D fighter respond. |

---

## Core Game Loop

```
Player speaks command → Gemini Live API processes → 
Backend maps to game action → Three.js plays animation → 
AI decides counter-move → AI fighter animates + speaks trash talk →
Repeat until KO
```

---

## Fighter Commands (Voice → Action Mapping)

### Attack Commands
| Voice Command | Action | Damage | Speed |
|---|---|---|---|
| "Punch" / "Jab" | Quick straight punch | Low (5) | Fast |
| "Hook" | Wide hook punch | Medium (10) | Medium |
| "Uppercut" | Rising uppercut | High (15) | Slow |
| "Kick" | Mid kick | Medium (10) | Medium |
| "Roundhouse" | Spinning kick | High (15) | Slow |
| "Combo" | Punch-punch-kick chain | High (20) | Slow |

### Defense Commands
| Voice Command | Action | Effect |
|---|---|---|
| "Block" / "Guard" | Raise guard | Reduces incoming damage 80% |
| "Dodge left" / "Dodge right" | Sidestep | Avoids attack entirely if timed |
| "Duck" | Crouch | Avoids high attacks |
| "Counter" | Parry + quick strike | High risk/reward — if timed right, big damage |

### Taunts & Special
| Voice Command | Action |
|---|---|
| "Taunt" | Fighter taunts (builds special meter) |
| "Special" / "Finish him" | Ultimate move (requires full meter) |

---

## AI Opponent Design

### Personality: "KRONOS"
- Trash-talking fight commentator personality
- Deep, cocky voice (use Gemini Live API voice: "Orus" or "Fenrir")
- Says things like:
  - "That all you got? My grandmother hits harder."
  - "Oh nice try! But not nice ENOUGH."
  - "You're getting predictable. Boring me here."
- **Adapts to player:** If player keeps punching, Kronos blocks more. If player is defensive, Kronos gets aggressive.
- **Barge-in:** Player can interrupt Kronos mid-taunt to land a surprise attack — this is a KEY Gemini Live API feature to showcase.

### AI Fight Logic
```
Simple state machine:
- AGGRESSIVE: favors attacks (when player HP > AI HP)
- DEFENSIVE: favors blocks/dodges (when AI HP is low)
- TAUNTING: trash talks between exchanges
- ADAPTING: tracks player's last 5 moves, counters patterns
```

---

## Health & Round System

- Each fighter: **100 HP**
- **Best of 3 rounds**
- Round timer: **60 seconds** (if time runs out, highest HP wins)
- Health bars displayed at top of screen
- Special meter fills with successful attacks and taunts

---

## Visual Style: 3D Low-Poly

- Clean geometric fighters with bold colors
- Player fighter: **Blue/Cyan** tones
- AI fighter (Kronos): **Red/Orange** tones
- Arena: Simple geometric ring with dramatic lighting
- Animations: Snappy, exaggerated (think Untitled Goose Game meets Street Fighter)
- Camera: Side view like classic fighting games, slight 3D angle

---

## Flow Assets Needed

Generate these in Google Flow / Veo / Imagen:

### Characters (generate as reference art, then model in Three.js)
1. Player fighter — low-poly humanoid, blue/cyan color scheme, fighting stance
2. Kronos (AI) — low-poly humanoid, red/orange, larger/bulkier, menacing stance

### Backgrounds
3. Arena floor — geometric hexagonal ring platform
4. Arena background — dark with neon accent lights, crowd silhouettes

### UI Elements
5. Health bar designs
6. "ROUND 1 / ROUND 2 / ROUND 3" splash screens
7. "KO!" splash graphic
8. "VOICEARENA" logo/title screen

### For Demo Video
9. Trailer clips showing gameplay moments
10. Title card / intro animation

---

## Sound Design

- **No music generation needed** — use royalty-free fight music
- Gemini Live API handles ALL voice (AI trash talk + player voice input)
- Pre-load sound effects:
  - Punch/kick impacts (3-4 variations)
  - Block sound
  - Dodge whoosh
  - Crowd cheering
  - Round bell ding
  - KO impact
  - Special move charge-up + release

All SFX can be sourced from freesound.org or mixkit.co (free, royalty-free).
