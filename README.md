# Antigravity Custom Theme Skill

Turn any static artwork, aesthetic mood, or **animated live wallpaper** into a high-performance, 60fps custom visual experience inside **Antigravity** — packaged as both a standalone cross-platform CDP theme engine and a drop-in **AI Agent Skill** (`SKILL.md`).

> Transform your Antigravity IDE into an immersive workspace with live video wallpapers, smart background RAM auto-pause, Claude-style typewriter text streaming, and glassmorphic UI palettes — all without modifying application binaries.

---

## ✨ Features

- **🎬 Live Animated Video Wallpapers (60fps H.264 / WebM):**
  - Hardware-accelerated HTML5 video wallpaper playback seamlessly integrated into the Antigravity shell.
  - In-memory `Blob` + `URL.createObjectURL` streaming: safely bypasses Chromium HTTPS/file URL security constraints without CORS issues.
  - Static WebP poster frame fallbacks for instantaneous rendering during launch.

- **⚡ Intelligent Background Auto-Pause (RAM & GPU Optimization):**
  - **Zero Background Resource Waste:** The video engine automatically pauses playback (`video.pause()`) when you switch to another window, tab, or application (`window.blur`, `!document.hasFocus()`), or when Antigravity is minimized (`document.visibilitychange`).
  - **Instant Seamless Resume:** Playback resumes instantly (`video.play()`) the moment focus returns to Antigravity (`window.focus`).
  - Halts hardware video decoding and canvas re-draws while Antigravity is running in the background, freeing CPU/GPU cycles and keeping RAM usage minimal.

- **🖋️ Claude-Style Typewriter Stream for AI Responses:**
  - **Authentic Ink-Flow Cascade:** Newly generated response words float into place with a subtle vertical rise (`translateY: 6px -> 0`), optical blur settling (`blur: 2.5px -> 0`), and gentle scale transition.
  - **Pulsing Accent Cursor:** A signature glowing typewriter cursor (`.agy-typewriter-cursor`) dynamically leads the active streaming stream and gently fades out upon completion.
  - **Adaptive Cadence:** Automatically balances streaming delay (12ms – 25ms per word) for a natural, silky reading pace.
  - **Zero DOM Disruption:** Preserves all Markdown syntax, clickable links, code blocks, tables, and KaTeX math formulas.

- **🚀 Zero Live Blur Architecture (True 60fps Scroll):**
  - Avoids performance-degrading CSS `backdrop-filter: blur()` on scrolling containers.
  - Uses pre-baked offscreen canvas textures (`syncPrebakedGlass`) and gradient scrims, ensuring butter-smooth 60fps/120fps scrolling even on large conversations.

- **🛡️ Split-Browser Safe (`data-has-browser-hole`):**
  - Automatically senses Antigravity's split embedded Chromium view and cuts a clean `evenodd` `clip-path` window so web previews and devtools never get dimmed, tinted, or obscured.

- **🎨 Live Interactive Settings Modal:**
  - Switch presets on the fly, upload custom static wallpapers or custom User/Agent circular avatars, adjust background dimming, and persist preferences automatically to `theme_config.json`.

---

## 🎭 Included Theme Presets

| Theme Name | Type | Description |
| :--- | :---: | :--- |
| **Reze Sakura Bloom** | 🎬 Live Video | Chainsaw Man dusk aesthetic with drifting cherry blossoms, rose quartz accents, and custom sakura filigree. |
| **Hu Tao Spirit Bloom** | 🎬 Live Video | Genshin Impact twilight mood with glowing butterfly spirits, warm cinnabar/gold accents, and ghost ornaments. |
| **Mitsuha Shooting Stars**| 🎬 Live Video | *Your Name* (Kimi no Na wa) night sky with shooting stars, twilight indigo and meteor violet glow. |
| **Blue Sea Sunset** | 🎬 Live Video | Serene anime twilight ocean sunset with golden hour turquoise waves and calming nautical accents. |
| **WLOP Cosmic** | 🖼️ Static 2K | Starlight violin illustration by WLOP on deep cosmic navy with royal gold flourishes. |
| **Midnight Obsidian** | ⬛ Pure CSS | Ultra-light zero-asset pure CSS gradient dark theme for minimalists. |

---

## 📋 Prerequisites

- **Antigravity** running with CDP debugging port active (`DevToolsActivePort`)
- **Node.js >= 22** (uses native `globalThis.WebSocket`, zero `npm install` needed)
- **Python 3.9+** (used to compile assets and payloads via `scripts/build_payload.py`)
- *(Optional)* **FFmpeg** if transcoding custom 4K videos into lightweight 1080p MP4s

---

## 🚀 Quick Start

### 1. Build the Runtime Payload

```bash
python scripts/build_payload.py
```

This resolves `themes/themes.json` + `themes/assets/*`, compresses images, encodes MP4 videos, and bundles everything with `engine/core.js` into the gitignored `dist/theme_payload.js`.

### 2. Install & Run the Theme Daemon

The daemon monitors `dist/theme_payload.js` and automatically hot-reloads Antigravity whenever you rebuild or modify a theme.

#### Windows (PowerShell):

```powershell
powershell -ExecutionPolicy Bypass -File scripts/install-win.ps1
```

- Registers dual persistent auto-start (Windows Startup folder + `HKCU\...\Run` registry key) so the daemon is always active on Windows logon.
- Launches the daemon detached via WMI so it survives Antigravity app restarts and process crashes.
- Automatically patches your Antigravity shortcuts with `--remote-debugging-port=0` if needed.
- Pass `-NoStartup` if you only want to test in the current session.

#### macOS / Linux (Bash):

```bash
bash scripts/install-unix.sh
```

- On **macOS**, registers a `~/Library/LaunchAgents/io.antigravity.custom-theme.plist` LaunchAgent.
- On **Linux**, registers a `systemd --user` service (or falls back to `nohup`).
- Pass `--foreground` to run interactively in the current terminal.

### 3. Verify Live via CDP

```bash
node scripts/verify_cdp.js
```

Runs diagnostic assertions against the active Antigravity window to verify that the theme is applied, video is playing, and zero live blur rules are violated.

### 4. Clean Uninstall

```bash
node scripts/uninstall.js
```

Stops the background daemon, removes startup entries, and immediately restores Antigravity's default styling.

---

## 🛠️ Adding Your Own Themes & Live Wallpapers

### Adding a Live Video Wallpaper

1. **Optimize your video:** Ensure your MP4/WebM is encoded efficiently:
   ```bash
   ffmpeg -i input.mp4 -vf "scale=1920:1080" -c:v libx264 -crf 23 -preset slow -an -movflags +faststart themes/assets/my_wallpaper_1080p.mp4
   ```
   *(Keep file size around 2–4MB for instantaneous in-memory loading).*

2. **Generate a poster image:**
   ```bash
   ffmpeg -ss 00:00:01 -i themes/assets/my_wallpaper_1080p.mp4 -vframes 1 -q:v 2 themes/assets/my_wallpaper_poster.webp
   ```

3. **Add the entry in `themes/themes.json`:**
   ```json
   {
     "My Custom Theme": {
       "primary": "#38bdf8",
       "secondary": "#818cf8",
       "glow": "rgba(56, 189, 248, 0.35)",
       "overlayRgb": "15, 23, 42",
       "wallpaper": "assets/my_wallpaper_poster.webp",
       "videoWallpaper": "assets/my_wallpaper_1080p.mp4",
       "bgPosition": "center center",
       "bg": "rgba(15, 23, 42, 0.88)",
       "frameBg": "linear-gradient(135deg, rgba(56, 189, 248, 0.75), rgba(129, 140, 248, 0.6))",
       "sidebarBg": "rgba(15, 23, 42, 0.72)",
       "aiCardBg": "rgba(30, 41, 59, 0.75)",
       "headerBg": "rgba(15, 23, 42, 0.78)"
     }
   }
   ```

4. **Recompile the payload:**
   ```bash
   python scripts/build_payload.py
   ```
   *The running daemon will immediately detect the file change and hot-reload Antigravity.*

---

## 🤖 Use as an Antigravity AI Skill (`SKILL.md`)

This repository is fully compliant with the Antigravity Skill specification. To use it as an autonomous agent skill:

1. Clone or copy this repository into:
   - **Global Skill Directory:** `~/.gemini/config/skills/antigravity-custom-theme/`
   - **Workspace Skill Directory:** `.agents/skills/antigravity-custom-theme/`
2. In your Antigravity chat, prompt your agent:
   > *"Use antigravity-custom-theme to create a dark cyberpunk theme with cyan/magenta accents and an animated wallpaper from this video."*
3. The agent will read `SKILL.md` + `references/theme-schema.md`, extract color tokens, stage files, build the payload, and verify live injection automatically.

---

## 🔍 Troubleshooting

### Theme Disappeared After an Antigravity Update

Starting with **Antigravity v2.33.0+**, Antigravity requires the `--remote-debugging-port=0` flag to expose its DevTools port. The built-in auto-updater may relaunch `Antigravity.exe` without shortcut parameters.

- **Windows:**
  1. Re-run `powershell -ExecutionPolicy Bypass -File scripts/install-win.ps1` (patches the flag into all Start Menu and Desktop shortcuts).
  2. **Quit Antigravity completely** (Right-click tray icon -> **Quit**).
  3. Reopen Antigravity from the **Start Menu**.
  4. If pinned to the **Taskbar**, unpin and re-pin the new shortcut from the Start Menu.
- **macOS / Linux:**
  - Launch with `--remote-debugging-port=0`:
    - macOS: `open -a Antigravity --args --remote-debugging-port=0`
    - Linux: Append `--remote-debugging-port=0` to the `Exec=` line in `~/.local/share/applications/antigravity.desktop`.

---

## 📂 Repository Layout

```text
antigravity-custom-theme-skill/
├── SKILL.md                    # Antigravity AI Agent Skill definition
├── README.md                   # Documentation & setup guide
├── LICENSE                     # MIT License
├── references/
│   └── theme-schema.md         # Full themes.json key reference & guidelines
├── themes/
│   ├── themes.json             # Theme definitions & palettes
│   └── assets/                 # Video wallpapers (.mp4), posters (.webp), avatars, SVGs
├── engine/
│   └── core.js                 # Core DOM injection engine, auto-pause & typewriter stream
├── daemon/
│   └── theme_daemon.js         # Cross-platform Node.js CDP injector & watcher
├── scripts/
│   ├── build_payload.py        # Compiles themes.json + core.js -> dist/theme_payload.js
│   ├── install-win.ps1         # Windows startup installation script
│   ├── install-unix.sh         # macOS LaunchAgent / Linux systemd installer
│   ├── uninstall.js            # Clean removal script
│   └── verify_cdp.js           # Live CDP test & verification script
└── dist/                       # Compiled runtime output (gitignored)
```

---

## 📄 License

Distributed under the [MIT License](LICENSE).
