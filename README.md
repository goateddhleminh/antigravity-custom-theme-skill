<div align="center">

# 🌌 Antigravity Custom Theme Engine

**Hardware-accelerated 60fps live animated wallpapers, zero-live-blur glassmorphic palettes, smart RAM auto-pause, and Claude-style typewriter streaming for the Antigravity IDE — with zero binary modifications.**

[![Release Version](https://img.shields.io/badge/version-2.4.0-blue.svg?style=for-the-badge&logo=semver)](https://github.com/goateddhleminh/antigravity-custom-theme-skill/releases)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen.svg?style=for-the-badge&logo=githubactions&logoColor=white)](https://github.com/goateddhleminh/antigravity-custom-theme-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D22.0-339933.svg?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB.svg?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey.svg?style=for-the-badge&logo=windows)](https://github.com/goateddhleminh/antigravity-custom-theme-skill)

<br />

<img src="./assets/demo.gif" width="85%" alt="Antigravity Custom Theme Live Demo" />

<br />

[ 🚀 Quick Start ](#-frictionless-quick-start-60s-setup) &nbsp;•&nbsp;
[ ✨ Core Features ](#-core-features) &nbsp;•&nbsp;
[ 🏗️ Architecture ](#%EF%B8%8F-tech-stack--architecture) &nbsp;•&nbsp;
[ 🎭 Presets ](#-included-theme-presets) &nbsp;•&nbsp;
[ 🤖 AI Agent Skill ](#-use-as-an-antigravity-ai-skill) &nbsp;•&nbsp;
[ 🐛 Report Bug ](https://github.com/goateddhleminh/antigravity-custom-theme-skill/issues)

</div>

---

## 💡 Why Antigravity Custom Theme?

The Antigravity IDE provides an exceptional coding and reasoning environment, but ships with rigid default dark/light appearances and lacks native support for live video wallpapers, bespoke glassmorphic aesthetics, or streaming response animations. 

Directly altering application binaries breaks auto-update mechanisms and violates integrity hashes. **Antigravity Custom Theme Engine** solves this by establishing a non-invasive, external Chrome DevTools Protocol (CDP) bridge that injects a self-contained, pre-baked rendering engine into the active IDE window in real time — surviving application restarts, system reboots, and official software updates seamlessly.

---

## ✨ Core Features

- **🎬 60fps Hardware-Accelerated Live Wallpapers**: In-memory `Blob` + `URL.createObjectURL` video streaming pipeline that bypasses Chromium local file security boundaries without CORS issues. Bundles static WebP poster fallbacks for instant, flicker-free startup rendering.
- **⚡ Intelligent Background Auto-Pause (RAM & GPU Saver)**: Zero idle resource waste. Automatically halts video decoding and GPU canvas repaints whenever Antigravity loses focus or is minimized, immediately reclaiming CPU/GPU resources and dropping background power consumption to near zero.
- **🖋️ Claude-Style Typewriter Stream**: Words dynamically cascade into view with an organic vertical rise (`translateY: 6px → 0`), optical blur settling (`blur: 2.5px → 0`), and an ambient pulsing cursor (`.agy-typewriter-cursor`) — preserving Markdown, syntax highlighting, code blocks, tables, and KaTeX formulas.
- **🛡️ Zero Live Blur Architecture (True 60fps Scroll)**: Eliminates performance-crippling CSS `backdrop-filter: blur()` on scrolling containers. Pre-bakes frosted glass textures via offscreen canvas buffers (`syncPrebakedGlass`) to guarantee smooth 60–120fps scrolling even in massive conversation histories.
- **🪟 Split-Browser Safe Cutouts**: Dynamically inspects the DOM for Antigravity's embedded Chromium browser view (`data-has-browser-hole`) and carves an exact `evenodd` `clip-path` window so live web previews, devtools, and sidecar panes remain crystal clear.
- **🔄 Detached Persistent Daemon & Hot Reload**: Operates independently of Antigravity's process tree via Windows WMI / macOS LaunchAgents / Linux systemd. Automatically senses `DevToolsActivePort`, re-injects on launch/restart, and hot-reloads themes whenever asset bundles are recompiled.

---

## 🎭 Included Theme Presets

| Theme Name | Type | Accent | Description |
| :--- | :---: | :---: | :--- |
| **Reze Sakura Bloom** | 🎬 Live Video | `#f472b6` | Chainsaw Man twilight aesthetic with drifting cherry blossoms, rose quartz glass, and custom sakura filigree. |
| **Hu Tao Spirit Bloom** | 🎬 Live Video | `#ef4444` | Genshin Impact twilight mood with glowing butterfly spirits, warm cinnabar/gold accents, and ghost ornaments. |
| **Mitsuha Shooting Stars**| 🎬 Live Video | `#818cf8` | *Your Name* (*Kimi no Na wa*) night sky with shooting stars, twilight indigo gradients, and meteor violet glow. |
| **Blue Sea Sunset** | 🎬 Live Video | `#38bdf8` | Serene anime twilight ocean sunset with golden hour turquoise waves and calming nautical accents. |
| **WLOP Cosmic** | 🖼️ Static 2K | `#fbbf24` | High-contrast starlight violin illustration by WLOP on deep cosmic navy with royal gold flourishes. |
| **Midnight Obsidian** | ⬛ Pure CSS | `#a78bfa` | Ultra-light zero-asset pure CSS gradient dark theme tailored for minimalists and low-spec systems. |

---

## 🏗️ Tech Stack & Architecture

### Technology Stack

| Layer | Technologies | Purpose |
| :--- | :--- | :--- |
| **Runtime Daemon** | Node.js (>= 22), native `WebSocket` | High-performance CDP automation client without third-party npm dependencies |
| **Compiler / Assets** | Python 3.9+, Pillow, FFmpeg | Automated raster compression to 2K WebP, base64 bundling, and MP4 optimization |
| **Injection Engine** | Vanilla ES6+, MutationObserver, RAF | Zero-overhead DOM orchestration, pre-baked canvas glass filters, and streaming text hooks |
| **Video Pipeline** | HTML5 `<video>`, Blob API, H.264/WebM | Low-latency in-memory video playback bypassing Chromium web security constraints |
| **OS Automation** | Windows WMI / VBScript, LaunchAgents, systemd | Detached, resilient background auto-start surviving process crashes and restarts |

### System Architecture & Data Flow

```mermaid
flowchart TD
    subgraph Antigravity ["Antigravity IDE (Chromium / Electron)"]
        CDP["DevToolsActivePort (CDP Endpoint)"]
        DOM["Antigravity DOM Shell (.agy-shell)"]
        Video["HTML5 Hardware-Accelerated Video Layer"]
        Typewriter["Claude-Style Typewriter Stream"]
        Hole["Split-Browser Safe Cutout (clip-path)"]
        Modal["Appearance Settings Modal"]
    end

    subgraph Daemon ["Theme Daemon (daemon/theme_daemon.js)"]
        PortWatcher["Port Scanner & Watcher (1.5s interval)"]
        WS["Native WebSocket CDP Client"]
        HotReload["Payload Watcher (dist/theme_payload.js)"]
        PIDLock["PID Lock Manager (daemon.pid)"]
    end

    subgraph Compiler ["Asset Compiler (scripts/build_payload.py)"]
        Themes["themes/themes.json + assets/*"]
        Core["engine/core.js"]
        Payload["dist/theme_payload.js (Self-Contained)"]
    end

    Themes --> Compiler
    Core --> Compiler
    Compiler --> Payload
    Payload -.->|Hot Reload Trigger| HotReload
    CDP -->|Port Discovery| PortWatcher
    PortWatcher --> WS
    HotReload --> WS
    WS -->|Page.addScriptToEvaluateOnNewDocument| DOM
    WS -->|Runtime.evaluate| DOM
    DOM --> Video
    DOM --> Typewriter
    DOM --> Hole
    Modal -.->|Runtime.bindingCalled (agySaveThemeConfig)| Daemon
    Daemon -.->|Persist Config| Config[theme_config.json]
```

---

## ⚡ Frictionless Quick Start (< 60s Setup)

### Prerequisites

- **Antigravity IDE** with CDP enabled (auto-configured by installer)
- **Node.js >= 22.0.0** (with built-in native `WebSocket` support)
- **Python >= 3.9** (with `Pillow` recommended for automated WebP compression)
- *(Optional)* **FFmpeg** (if encoding custom video wallpapers)

### 1. Clone the Repository

```bash
git clone https://github.com/goateddhleminh/antigravity-custom-theme-skill.git
cd antigravity-custom-theme-skill
```

### 2. Build the Theme Payload

Compile `themes/themes.json` and `engine/core.js` into the self-contained bundle:

```bash
python scripts/build_payload.py
```

### 3. Install & Start the Persistent Daemon

#### Windows (PowerShell):
```powershell
powershell -ExecutionPolicy Bypass -File scripts/install-win.ps1
```
*Configures dual auto-start (`HKCU\...\Run` + `Startup` folder), patches `--remote-debugging-port=0` onto Antigravity shortcuts, and launches the daemon detached via WMI.*

#### macOS / Linux (Bash):
```bash
bash scripts/install-unix.sh
```
*Registers a user-level `LaunchAgent` (macOS) or `systemd --user` unit (Linux) and starts the daemon in the background.*

### 4. Verify Active Injection

Confirm that the theme is applied and zero performance rules are violated:

```bash
node scripts/verify_cdp.js
```

### 5. Clean Uninstall

To completely revert to default Antigravity styling and remove all background services:

```bash
node scripts/uninstall.js
```

---

## ⚙️ Environment Variables

The theme daemon and scripts accept the following optional environment variables:

| Variable | Type | Description | Default |
| :--- | :---: | :--- | :--- |
| `AGY_PORT_FILE` | `String` | Absolute path to the custom Antigravity `DevToolsActivePort` file | Auto-detected per OS |
| `AGY_CONFIG_PATH` | `String` | Custom path to the user theme preference configuration JSON file | `./theme_config.json` |
| `AGY_DAEMON_STDOUT` | `String` | Set to `1` to mirror all daemon log output to stdout in non-TTY sessions | `0` (Writes to `daemon.log`) |

---

## 🛠️ Adding Your Own Themes & Live Wallpapers

Adding new static or video wallpapers takes less than 2 minutes:

1. **Optimize your video wallpaper (1080p, 30fps, 2–4MB target)**:
   ```bash
   ffmpeg -i my_video.mp4 -vf "scale=1920:1080" -c:v libx264 -crf 24 -preset medium -an -movflags +faststart themes/assets/my_theme_1080p.mp4
   ```

2. **Extract a fallback poster frame**:
   ```bash
   ffmpeg -ss 00:00:01 -i themes/assets/my_theme_1080p.mp4 -vframes 1 -q:v 2 themes/assets/my_theme_poster.webp
   ```

3. **Declare the theme in `themes/themes.json`**:
   ```json
   {
     "My Custom Theme": {
       "concept": "Minimalist Cyberpunk • Neon Cyan & Midnight Glass",
       "primary": "#06b6d4",
       "secondary": "#3b82f6",
       "glow": "rgba(6, 182, 212, 0.35)",
       "overlayRgb": "10, 15, 26",
       "wallpaper": "assets/my_theme_poster.webp",
       "videoWallpaper": "assets/my_theme_1080p.mp4",
       "bgPosition": "center center",
       "bg": "linear-gradient(145deg, rgba(10, 15, 26, 0.90) 0%, rgba(5, 8, 15, 0.96) 100%)",
       "frameBg": "linear-gradient(135deg, rgba(6, 182, 212, 0.8), rgba(59, 130, 246, 0.6))",
       "sidebarBg": "rgba(10, 15, 26, 0.30)",
       "aiCardBg": "rgba(15, 23, 42, 0.75)",
       "headerBg": "rgba(10, 15, 26, 0.30)"
     }
   }
   ```

4. **Recompile the bundle**:
   ```bash
   python scripts/build_payload.py
   ```
   *The active daemon automatically detects the updated bundle and reloads your Antigravity window instantaneously.*

---

## 🤖 Use as an Antigravity AI Skill

This repository adheres directly to the Antigravity Skill specification (`SKILL.md`). You can install it into your agent's skill library:

1. Link or clone into your skills directory:
   - **Global Skill Directory**: `~/.gemini/config/skills/antigravity-custom-theme/`
   - **Workspace Directory**: `.agents/skills/antigravity-custom-theme/`
2. Command your agent naturally:
   > *"Use antigravity-custom-theme to generate a dark luxury aesthetic theme using an electric emerald palette and this uploaded video wallpaper."*
3. The agent will read `SKILL.md` and `references/theme-schema.md`, extract primary color tokens, compile the payload, and verify CDP injection autonomously.

---

## 📂 Project Directory Tree

```text
antigravity-custom-theme-skill/
├── SKILL.md                    # Antigravity AI Agent Skill declaration & guidelines
├── README.md                   # Comprehensive architectural documentation & user guide
├── LICENSE                     # MIT Open Source License
├── theme_config.json           # Active user theme state & runtime preferences
├── references/
│   └── theme-schema.md         # Full themes.json key reference & styling guidelines
├── themes/
│   ├── themes.json             # Theme definitions, color palettes & asset mappings
│   └── assets/                 # Video wallpapers (.mp4), posters (.webp), avatars, SVGs
├── engine/
│   └── core.js                 # Core DOM injection engine, RAM auto-pause & typewriter stream
├── daemon/
│   └── theme_daemon.js         # Cross-platform Node.js (>= 22) CDP watcher & hot reloader
├── scripts/
│   ├── build_payload.py        # Compiles themes.json + core.js -> dist/theme_payload.js
│   ├── install-win.ps1         # Windows dual-boot persistent installer (WMI + Registry)
│   ├── install-unix.sh         # macOS LaunchAgent & Linux systemd installer
│   ├── uninstall.js            # Clean removal script restoring default styling
│   └── verify_cdp.js           # Headless CDP test runner & zero-blur verifier
└── dist/                       # Compiled runtime output bundle (gitignored)
    └── theme_payload.js
```

---

## 🗺️ Roadmap & Milestones

- [x] **Zero Live Blur Architecture**: High-speed pre-baked canvas glass textures.
- [x] **Hardware-Accelerated HTML5 Video**: In-memory Blob streaming for 60fps live wallpapers.
- [x] **Smart RAM Auto-Pause**: Dynamic activity tracking pausing playback when unfocused or minimized.
- [x] **Claude-Style Typewriter Engine**: Organic flowing cascade with pulsing accent cursor for AI text.
- [x] **Permanent Windows & Unix Persistence**: Independent detached daemons surviving app updates.
- [x] **Split-Browser Protection**: Automatic `clip-path` masking for integrated Chromium preview views.
- [ ] **Audio-Reactive Visualizers**: Dynamic particle fields reacting to system audio frequencies.
- [ ] **Interactive Theme Builder GUI**: Real-time in-app palette editor and CSS variable playground.
- [ ] **Community Theme Registry**: Direct CLI command to browse and install themes from the cloud.

---

## 🤝 Contributing

Contributions from the developer and design community are enthusiastically welcomed!

1. **Fork the Repository**: Click the `Fork` button at the top right of this repository.
2. **Create a Feature Branch**:
   ```bash
   git checkout -b feature/amazing-theme
   ```
3. **Commit Your Changes**: Follow Conventional Commits:
   ```bash
   git commit -m "feat(themes): add Synthwave Sunset live wallpaper theme"
   ```
4. **Push to the Branch**:
   ```bash
   git push origin feature/amazing-theme
   ```
5. **Open a Pull Request**: Submit your PR with a screenshot or short video preview!

---

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for complete details.

---

<div align="center">
  <sub>Engineered with precision for the modern Antigravity workspace. Built by developers, for developers.</sub>
</div>
