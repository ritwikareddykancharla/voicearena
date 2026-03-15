# VOICEARENA — Veo3 Video Prompts (Realistic MMA Fighters)

## Strategy: Pre-Generated Video Clips with First/Last Frame Anchoring

1. Generate an **idle pose IMAGE** for each character in Imagen first
2. Use that image as the **first AND last frame** for every Veo3 video clip
3. Each clip = 4 seconds (shortest, fastest to generate, snappiest in-game)
4. During gameplay, swap between video clips with transition effects
5. Idle clip loops continuously, action clips play once then return to idle

---

## CHARACTER DESCRIPTIONS

### BLADE (Player)
- Mid-20s, lean athletic build, defined muscles
- Short dark hair with a pointed fringe
- Shirtless, showing fight-ready physique
- Blue MMA fight shorts with white stripe down the side
- Blue hand wraps on both fists, taped knuckles
- Barefoot
- A small tattoo on his left shoulder (tribal or geometric)

### KRONOS (AI Opponent)
- Late 30s, big and jacked, heavyweight build
- Shaved head, thick short dark beard
- Scar across left eyebrow
- Shirtless, covered in tattoos across chest and arms (sleeve tattoos)
- Red and black MMA fight shorts
- Red hand wraps, taped knuckles
- Barefoot
- Looks like he's been in 100 fights and won them all

---

## STEP 1: Generate Idle Frame Images (Imagen/Flow)

These are your anchor images. Every video starts and ends on this frame.

### BLADE — Idle Frame
```
Photorealistic young athletic shirtless MMA fighter in his mid-20s, 
lean muscular build with defined abs, short dark hair with pointed fringe, 
small geometric tattoo on left shoulder, wearing blue MMA fight shorts 
with white stripe, blue hand wraps with taped knuckles, barefoot, 
standing in an orthodox MMA fighting stance facing right, 
fists raised at chin level, weight on back foot ready to strike, 
dark moody MMA arena background with blue rim lighting, 
cinematic portrait, side view, full body visible head to toe, 
professional UFC fight photography style, 8K detail
```

### KRONOS — Idle Frame
```
Photorealistic tall heavily muscular shirtless MMA fighter in his late 30s, 
heavyweight build with thick arms and broad chest covered in sleeve tattoos, 
shaved head, thick short dark beard, scar across left eyebrow, 
wearing red and black MMA fight shorts, red hand wraps with taped knuckles, 
barefoot, standing in a southpaw MMA fighting stance facing left, 
fists up, chin tucked, cold intimidating death stare, 
dark moody MMA arena background with red rim lighting, 
cinematic portrait, side view, full body visible head to toe, 
professional UFC fight photography style, 8K detail
```

---

## STEP 2: Generate Video Clips (Veo3)

**Settings for ALL clips:**
- Duration: **4 seconds**
- Resolution: **720p**
- Aspect ratio: **16:9**
- First frame: character's idle image
- Last frame: same idle image
- Generate audio: **OFF**

---

## BLADE (Player) — Video Prompts

### Idle (looping)
```
The shirtless MMA fighter in blue shorts subtly shifts weight between 
feet in his fighting stance, fists bobbing at chin level, 
breathing visible through his defined abs, shoulder muscles tense, 
head slightly moving tracking his opponent, barefoot on canvas floor, 
cinematic side view, dark MMA arena with blue lighting, 
smooth professional UFC broadcast camera feel
```

### Punch / Jab
```
The shirtless MMA fighter in blue shorts snaps a fast straight right jab, 
fist shooting forward from his chin, arm fully extending with speed, 
shoulder rotating into the strike, sweat flicking off his arm, 
then quickly snaps back to fighting stance, 
cinematic side view, dark MMA arena with blue lighting, 
high speed UFC camera feel
```

### Hook
```
The shirtless MMA fighter in blue shorts throws a devastating right hook, 
torso rotating powerfully, bare arm sweeping in a wide arc at jaw height, 
his whole body weight behind the punch, muscles flexing on impact, 
then recovers back to stance, cinematic side view, 
dark MMA arena with blue lighting, slow motion UFC replay feel
```

### Uppercut
```
The shirtless MMA fighter in blue shorts drops low bending his knees 
then explosively drives a right uppercut upward, fist rising from 
waist to above head height, legs extending with the punch, 
powerful rising motion visible through his core muscles, 
then settles back to fighting stance, cinematic side view, 
dark MMA arena with blue lighting, dramatic slow motion
```

### Kick
```
The shirtless MMA fighter in blue shorts launches a fast right 
roundhouse kick to the body, bare leg swinging up and connecting 
at ribcage height, hip rotating through, standing leg planted firmly, 
sharp impactful movement, leg snaps back and returns to fighting stance, 
cinematic side view, dark MMA arena with blue lighting, 
professional UFC broadcast camera
```

### Roundhouse Kick (Head)
```
The shirtless MMA fighter in blue shorts spins his body and delivers 
a massive spinning heel kick, full 360 rotation with right leg 
sweeping at head height, athletic and explosive, 
body completes the spin and returns to fighting stance, 
cinematic side view, dark MMA arena with blue lighting, 
slow motion dramatic UFC highlight reel
```

### Block / Guard
```
The shirtless MMA fighter in blue shorts quickly tucks his chin and 
raises both forearms tight against his head and body in a shell guard, 
absorbing an incoming strike, body bracing on impact, 
slight pushback from the force, holds the guard briefly 
then returns to fighting stance, cinematic side view, 
dark MMA arena with blue lighting, professional fight footage
```

### Dodge Left
```
The shirtless MMA fighter in blue shorts swiftly slips to the left, 
head moving off the center line, feet shuffling laterally, 
smooth evasive head movement narrowly avoiding a punch, 
then resets back to center fighting stance, cinematic side view, 
dark MMA arena with blue lighting, professional fight footage
```

### Dodge Right
```
The shirtless MMA fighter in blue shorts swiftly slips to the right, 
head moving off the center line, feet shuffling laterally, 
smooth evasive head movement narrowly avoiding a punch, 
then resets back to center fighting stance, cinematic side view, 
dark MMA arena with blue lighting, professional fight footage
```

### Duck
```
The shirtless MMA fighter in blue shorts quickly level changes, 
dropping low into a crouch bending at the knees, 
a punch whooshes over his head missing completely, 
then he springs back up to fighting stance fast and agile, 
cinematic side view, dark MMA arena with blue lighting, 
professional fight footage
```

### Taking a Hit
```
The shirtless MMA fighter in blue shorts gets cracked with a hard 
punch to the jaw, head snapping to the side from impact, 
sweat spraying off his face, mouthguard visible, pain expression, 
body jolting backward, legs wobbling slightly, 
then regains fighting stance, cinematic side view, 
dark MMA arena with blue lighting, high speed impact camera
```

### KO / Falling
```
The shirtless MMA fighter in blue shorts takes a massive knockout punch, 
head snapping back violently, eyes rolling, body going completely limp, 
falling backward in slow motion like a tree, arms dropping to sides, 
back hitting the canvas floor with a thud, lying flat defeated, 
cinematic side view, dark MMA arena with blue lighting, 
dramatic slow motion UFC knockout replay
```

### Victory Celebration
```
The shirtless MMA fighter in blue shorts raises both fists in the air, 
screaming in triumph, veins visible in his neck and arms, 
then climbs on the cage fence with one foot and beats his chest, 
intense victorious expression, sweat dripping, 
cinematic side view, dark MMA arena with blue lights intensifying, 
UFC championship celebration moment
```

### Special Move
```
The shirtless MMA fighter in blue shorts bounces on his feet, 
then explodes forward with a flying superman punch, 
leaping through the air with right fist extended, 
full body airborne for a moment, devastating impact on landing, 
slides back to fighting stance, cinematic side view, 
dark MMA arena with blue lighting, dramatic slow motion speed ramp
```

---

## KRONOS (AI Opponent) — Video Prompts

### Idle (looping)
```
The large heavily tattooed shirtless MMA fighter in red shorts 
shifts weight menacingly, rolling his thick neck slowly, 
clenching and unclenching his taped fists, cold death stare 
fixed forward, jaw clenched under his dark beard, barely contained 
aggression, cinematic side view, dark MMA arena with red lighting, 
professional fight footage
```

### Punch
```
The large tattooed shirtless MMA fighter in red shorts steps forward 
and throws a brutal straight left cross, heavy and powerful, 
whole body weight behind it, thick arm fully extending with force, 
then pulls back to stance, cinematic side view, 
dark MMA arena with red lighting, heavy impact feel
```

### Hook
```
The large tattooed shirtless MMA fighter in red shorts throws a 
massive left hook, thick arm sweeping at head height like a battering ram, 
his full heavyweight body rotating into the punch, devastating power, 
then recovers to stance, cinematic side view, 
dark MMA arena with red lighting, professional fight footage
```

### Uppercut
```
The large tattooed shirtless MMA fighter in red shorts dips low 
and launches a crushing uppercut, left fist driving upward with 
explosive force, massive legs propelling the punch, 
then settles back to fighting stance, cinematic side view, 
dark MMA arena with red lighting, slow motion power shot
```

### Kick
```
The large tattooed shirtless MMA fighter in red shorts throws a 
heavy left leg kick to the body, thick bare leg swinging with 
crushing force, brutal and heavy enough to crack ribs, 
leg returns to stance, cinematic side view, 
dark MMA arena with red lighting, professional fight footage
```

### Roundhouse
```
The large tattooed shirtless MMA fighter in red shorts spins and 
delivers a devastating spinning back kick, full body rotation, 
left heel connecting with knockout force at head height, 
lands back in stance, cinematic side view, 
dark MMA arena with red lighting, slow motion dramatic
```

### Block
```
The large tattooed shirtless MMA fighter in red shorts raises both 
thick forearms into a tight high guard, absorbing a hit with barely 
any reaction, unmoved, slight smirk under his beard while blocking, 
then drops guard back to fighting stance, cinematic side view, 
dark MMA arena with red lighting, professional fight footage
```

### Dodge
```
The large tattooed shirtless MMA fighter in red shorts leans back 
casually slipping a punch, minimal head movement, making it look 
effortless, almost disrespectful evasion, slight smirk, 
then returns to fighting stance, cinematic side view, 
dark MMA arena with red lighting, professional fight footage
```

### Taking a Hit
```
The large tattooed shirtless MMA fighter in red shorts gets caught 
with a hard shot to the face, head turning from the impact, 
genuine surprise on his face, sweat flying, body absorbing the blow, 
slight stumble, then regains stance with furious angry expression, 
cinematic side view, dark MMA arena with red lighting, 
high speed impact camera
```

### KO
```
The large tattooed shirtless MMA fighter in red shorts takes a 
devastating knockout blow, eyes going wide with shock, mouthguard 
flying out, body stiffening, falling forward face first in slow motion, 
crashing to the canvas, lying motionless and defeated, 
cinematic side view, dark MMA arena with red lighting, 
dramatic slow motion knockout replay
```

### Victory
```
The large tattooed shirtless MMA fighter in red shorts stands over 
his fallen opponent, arms spread wide looking down with cold contempt, 
then slowly crosses his arms over his tattooed chest, 
slight shake of his head in disappointment, red arena lights 
intensifying behind him, cinematic side view, dark MMA arena, 
intimidating villain victory moment
```

### Taunt
```
The large tattooed shirtless MMA fighter in red shorts drops his hands 
completely to his sides, chin up exposed, beckoning the opponent 
forward with one hand, cocky smirk under his beard, 
completely relaxed and disrespectful, other hand gesturing 
to come and hit him, cinematic side view, dark MMA arena red lighting, 
professional fight footage
```

### Special Move
```
The large tattooed shirtless MMA fighter in red shorts cracks his neck, 
then rushes forward and launches a brutal flying knee strike, 
knee driving forward with devastating force, whole body airborne, 
massive impact, lands and slides back to stance, 
cinematic side view, dark MMA arena with red lighting, 
dramatic slow motion with speed ramp
```

---

## ARENA BACKGROUND (Static Image — Imagen)

```
Photorealistic professional MMA octagon arena interior, side view, 
dark arena with canvas floor showing blood stains and scuff marks, 
black chain link cage walls, packed crowd visible but blurred 
behind the cage, dramatic overhead spotlights creating harsh 
pools of white light on the canvas, blue LED accent light strip 
on the left side and red LED accent light strip on the right side, 
atmospheric haze and fog catching the light, 
UFC championship fight atmosphere, 
wide 16:9 cinematic composition, professional broadcast camera angle, 
no fighters in the octagon, empty and ready for battle, 8K detail
```

---

## GENERATION ORDER (Priority)

**Batch 1 — PLAYABLE DEMO (generate these first, ~45 min):**
1. Blade idle image (Imagen)
2. Kronos idle image (Imagen)
3. Arena background (Imagen)
4. Blade idle video (Veo3)
5. Kronos idle video (Veo3)
6. Blade punch video
7. Kronos punch video
8. Blade hit video
9. Kronos hit video

**Batch 2 — CORE MOVES (~45 min):**
10. Blade block, kick, uppercut videos
11. Kronos block, kick, uppercut videos
12. Blade dodge left, dodge right videos
13. Kronos dodge, taunt videos

**Batch 3 — POLISH (~30 min):**
14. Blade roundhouse, duck, special videos
15. Kronos roundhouse, special videos
16. Blade KO, victory videos
17. Kronos KO, victory videos

---

## FILE NAMING

```
assets/videos/
├── blade_idle.mp4
├── blade_punch.mp4
├── blade_hook.mp4
├── blade_uppercut.mp4
├── blade_kick.mp4
├── blade_roundhouse.mp4
├── blade_block.mp4
├── blade_dodge_left.mp4
├── blade_dodge_right.mp4
├── blade_duck.mp4
├── blade_hit.mp4
├── blade_ko.mp4
├── blade_victory.mp4
├── blade_special.mp4
├── kronos_idle.mp4
├── kronos_punch.mp4
├── kronos_hook.mp4
├── kronos_uppercut.mp4
├── kronos_kick.mp4
├── kronos_roundhouse.mp4
├── kronos_block.mp4
├── kronos_dodge.mp4
├── kronos_hit.mp4
├── kronos_ko.mp4
├── kronos_victory.mp4
├── kronos_special.mp4
├── kronos_taunt.mp4
└── arena_bg.png
```
