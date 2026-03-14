# VOICEARENA — Flow / Imagen Asset Prompts

Use these prompts in Google Flow or Imagen to generate the visual assets for the game.

---

## Character Reference Art

### Player Fighter (Blue)
```
Low-poly 3D character design, fighting stance, humanoid male fighter, 
blue and cyan color scheme, geometric angular shapes, minimal detail, 
clean edges, white background, game character concept art, front view, 
T-pose reference sheet, Untitled Goose Game art style meets Street Fighter
```

### Kronos — AI Fighter (Red)
```
Low-poly 3D character design, fighting stance, large intimidating humanoid fighter, 
red and orange color scheme with glowing eyes, geometric angular shapes, 
bulkier than average, spiky shoulder details, minimal detail, clean edges, 
white background, game boss character concept art, front view, menacing pose
```

---

## Arena / Background

### Arena Floor
```
Top-down view of a hexagonal fighting arena platform, dark metallic surface 
with glowing blue and red accent lines along the edges, futuristic minimal 
design, low-poly geometric style, dramatic lighting from above, game asset
```

### Arena Background (Skybox)
```
Dark futuristic arena interior, neon lights in blue and red, crowd silhouettes 
in the background, volumetric lighting, fog, geometric architecture, 
low-poly style, fighting game arena, dramatic atmosphere, 16:9 aspect ratio
```

---

## UI Elements

### Title Screen / Logo
```
"VOICEARENA" text logo, bold angular futuristic font, metallic silver 
with blue and red glow effects, fighting game title screen style, 
dark background, dramatic lighting, sparks, clean vector design
```

### Round Splash Screens
```
"ROUND 1" text in bold metallic 3D letters, centered, dark background 
with blue and red lens flares, fighting game style, dramatic, clean
```

```
"KO" text in massive bold red 3D letters, explosion effects behind it, 
sparks flying, fighting game knockout screen, dramatic impact, dark background
```

```
"FIGHT!" text in bold white metallic 3D letters with orange glow, 
centered, dark arena background, fighting game round start, energetic
```

### Health Bar Design
```
Futuristic health bar UI element for a fighting game, horizontal bar, 
left side blue gradient, right side red gradient, metallic frame, 
angular design, transparent background, game UI asset, clean vector style
```

### Voice Indicator
```
Minimalist microphone icon with sound wave rings emanating from it, 
glowing cyan blue, dark transparent background, game UI element, 
pulsing animation frame, futuristic style
```

---

## For Demo Video / Thumbnail

### Thumbnail
```
Two low-poly 3D fighters facing off, one blue one red, dramatic lighting 
between them, "VOICEARENA" text above, dark futuristic arena background, 
fighting game poster style, microphone icon between them, 16:9 aspect ratio
```

### Video Intro Card
```
Dark cinematic frame with text "COMMAND YOUR FIGHTER WITH YOUR VOICE", 
futuristic font, blue and red accent lighting, subtle particle effects, 
fighting game aesthetic, 16:9, clean and dramatic
```

---

## Texture Maps (for Three.js materials)

### Arena Floor Texture
```
Seamless tileable dark metal floor texture with subtle hexagonal grid pattern, 
scratched metal surface, futuristic, slight blue reflection, game texture asset, 
1024x1024, top-down view
```

### Fighter Body Texture - Blue
```
Seamless low-poly character skin texture, blue and cyan gradient, 
subtle geometric panel lines, futuristic armor feel, flat shading reference, 
game texture asset, 512x512
```

### Fighter Body Texture - Red
```
Seamless low-poly character skin texture, red and orange gradient, 
subtle geometric panel lines with glowing cracks, menacing armor feel, 
flat shading reference, game texture asset, 512x512
```

---

## Notes on Using Flow

1. Generate multiple variations of each asset (4x) and pick the best
2. Use Imagen for static UI/texture assets — faster than Veo
3. For the title screen and splash screens, you can also just build them in CSS — might be cleaner
4. Export all assets at 2x resolution for sharpness
5. Use transparent backgrounds (PNG) where possible for easy compositing
6. The character reference art is for YOUR reference when building the Three.js models — you won't use the images directly in-game since fighters are built from code
