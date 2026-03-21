# FLAP.AI × IoT

**The Human-First Evolved Flappy Bird**  
GDG × Hack2Skill · Version 2.0

A reimagined Flappy Bird that knows you're playing it — your stress level, your room's lighting, your heart rate. Built with Gemini AI and physical IoT sensors.

## Quick Start

```bash
# No build step needed — pure HTML5 + vanilla JS
# Option 1: Open directly
open index.html

# Option 2: Use a local server (recommended for ES modules)
npx serve .
# Then visit http://localhost:3000
```

### Enable Gemini AI (Optional)

Set your API key via browser console:
```js
localStorage.setItem('flapai_gemini_key', 'YOUR_GEMINI_API_KEY');
```
The game works fully without Gemini — it uses handcrafted fallback worlds.

## Architecture

```
FLAP.AI/
├── index.html              One canvas. No framework.
├── src/
│   ├── main.js             Entry point, canvas setup
│   ├── game.js             60fps loop, state machine (4 states)
│   ├── renderer.js         All canvas drawing, parallax, effects
│   ├── sprites.js          Pixel art data, programmatic drawing
│   ├── physics.js          Constants, bird/pipe physics
│   ├── collision.js        Bitmap mask collision system
│   ├── audio.js            Web Audio API synthesized sounds
│   ├── input.js            Keyboard, touch, IoT, voice, face
│   ├── gemini.js           Gemini API client, rate limiter
│   ├── narrator.js         Voice narrator, event queue
│   ├── shadow.js           Shadow Faby AI ghost bird
│   ├── world.js            World themes, chapters
│   ├── iot.js              WebSocket IoT client
│   ├── biometric.js        Heart rate / GSR processing
│   ├── environment.js      Light / noise / motion sensing
│   └── ui.js               HUD, toasts, score persistence
├── firmware/
│   └── controller.ino      ESP32 Arduino firmware
├── bridge.py               Python serial → WebSocket bridge
├── .env.example            API key template
└── README.md               This file
```

## Controls

| Input | Action |
|-------|--------|
| Space / ↑ | Flap |
| Click / Tap | Flap |
| ESP32 Button | Flap (hold = shield, double = turbo) |
| Head nod (Face Mode) | Flap |
| Voice: "flap" | Flap |

## Gemini AI Integrations

1. **World Generator** — Generates unique world themes (colors, bird lore, chapters) before each run
2. **Narrator** — Watches gameplay and reacts with voice commentary
3. **Shadow Faby** — Ghost bird that learns from your mistakes and shows a better path
4. **Face-Flap Mode** — Control the bird with head movements via MediaPipe FaceMesh

## IoT Hardware Setup

### Components
| Part | Pin | Purpose |
|------|-----|---------|
| Push button | GPIO 4 | Flap input |
| Joystick (KY-023) | GPIO 34/35 | Force modulation |
| MAX30102 pulse | I2C (21/22) | Heart rate |
| BH1750 light | I2C (21/22) | Ambient light |
| Microphone | GPIO 34 | Noise level |
| PIR sensor (HC-SR501) | GPIO 5 | Motion detection |

### Wiring Diagram

```
ESP32 DevKit V1
┌─────────────────┐
│             3V3 ├──── Sensors VCC
│             GND ├──── Sensors GND
│          GPIO 4 ├──── Button (10kΩ pull-up)
│         GPIO 34 ├──── Joystick VRx / Mic OUT
│         GPIO 35 ├──── Joystick VRy
│         GPIO 32 ├──── Joystick SW
│         GPIO 21 ├──── I2C SDA (MAX30102 + BH1750)
│         GPIO 22 ├──── I2C SCL (MAX30102 + BH1750)
│          GPIO 5 ├──── PIR OUT
│             5V  ├──── PIR VCC
└─────────────────┘
```

### Running the IoT Bridge

```bash
pip install websockets pyserial
python bridge.py              # Auto-detect port
python bridge.py --port COM3  # Windows
```

## Game Physics

| Constant | Value | Notes |
|----------|-------|-------|
| Gravity | 0.25 | Original feel |
| Flap Force | -4.6 | Sharp upward pop |
| Max Fall | 8.0 | Terminal velocity cap |
| Pipe Speed | 2.4 (base) | Increases with score |
| Canvas | 288×512 | Native resolution |

## License

Built for GDG × Hack2Skill hackathon. Made with ❤ and caffeine.
