# GitHub Setup for VoiceArena

## Step 1: Create the repo on GitHub

1. Go to **github.com/new**
2. Repository name: `voicearena`
3. Description: `Voice-controlled 3D fighting game powered by Gemini Live API`
4. Set to **Public** (required for hackathon submission)
5. Check **Add a README file**
6. Click **Create repository**

## Step 2: Clone and set up locally

```bash
git clone https://github.com/YOUR_USERNAME/voicearena.git
cd voicearena
```

## Step 3: Create the project structure

```bash
# Create all folders
mkdir -p backend frontend/{js,css,assets/{sounds,textures,ui},lib} deploy/terraform docs

# Move the blueprint docs into /docs
# (copy the 5 .md files you downloaded into the docs/ folder)
```

Your folder should look like this:
```
voicearena/
├── README.md
├── docs/
│   ├── 01-GAME-DESIGN.md
│   ├── 02-ARCHITECTURE.md
│   ├── 03-BUILD-GUIDE.md
│   ├── 04-FLOW-ASSET-PROMPTS.md
│   └── 05-DEVPOST-SUBMISSION.md
├── backend/
├── frontend/
│   ├── js/
│   ├── css/
│   ├── assets/
│   │   ├── sounds/
│   │   ├── textures/
│   │   └── ui/
│   └── lib/
└── deploy/
    └── terraform/
```

## Step 4: Push it

```bash
git add .
git commit -m "Initial project structure + game design docs"
git push origin main
```

## Step 5: As you build, commit often

```bash
# After finishing a feature
git add .
git commit -m "Add Three.js scene + low-poly fighters"
git push

# Next feature
git add .
git commit -m "Add game engine + WebSocket backend"
git push

# And so on...
```

## Recommended commit order (matches the build guide)

1. `Initial project structure + game design docs`
2. `Add Three.js scene + low-poly fighters + idle animation`
3. `Add fight animations (punch, kick, block, hit)`
4. `Add game engine (HP, rounds, damage calc)`
5. `Add FastAPI backend + WebSocket`
6. `Add AI fight logic (Kronos brain)`
7. `Add Gemini Live API voice integration`
8. `Add ADK agent for Kronos`
9. `Add UI polish (HP bars, round screens, particles)`
10. `Add Dockerfile + Cloud Run deployment`
11. `Add architecture diagram + submission assets`

## .gitignore

Create a `.gitignore` file in the root:

```
# Python
__pycache__/
*.pyc
venv/
.env

# Node
node_modules/

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/

# Secrets
*.key
*.pem
service-account.json
```
