# Technical Documentation
## Personal AI Assistant - Desktop & Mobile

---

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [System Requirements](#system-requirements)
3. [Project Structure](#project-structure)
4. [Build Instructions](#build-instructions)
5. [API Reference](#api-reference)
6. [Third-Party Dependencies](#third-party-dependencies)
7. [Security Model](#security-model)
8. [Deployment Guide](#deployment-guide)
9. [Maintenance & Updates](#maintenance--updates)

---

## Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER INTERFACE                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────┐              ┌──────────────────┐         │
│  │  Desktop App     │◄────────────►│   Mobile App     │         │
│  │  (Windows/Rust)  │   WebSocket  │   (Android/      │         │
│  │                  │   Port 8765  │    Kotlin)       │         │
│  └────────┬─────────┘              └────────┬─────────┘         │
│           │                                  │                   │
├───────────┼──────────────────────────────────┼───────────────────┤
│           │         CORE SERVICES            │                   │
│           ▼                                  ▼                   │
│  ┌──────────────────┐              ┌──────────────────┐         │
│  │ System Control   │              │ Voice Service    │         │
│  │ - Input (mouse)  │              │ - TTS Engine     │         │
│  │ - Audio (volume) │              │ - STT Engine     │         │
│  │ - Browser        │              │ - Edge TTS       │         │
│  │ - Commands       │              │                  │         │
│  └──────────────────┘              └──────────────────┘         │
│                                                                  │
│  ┌──────────────────┐              ┌──────────────────┐         │
│  │ AI Service       │              │ Sync Service     │         │
│  │ - Gemini API     │              │ - Knowledge      │         │
│  │ - Groq API       │              │ - Conversation   │         │
│  │ - Local Ollama   │              │ - Settings       │         │
│  └──────────────────┘              └──────────────────┘         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Communication Protocol

Desktop and Mobile communicate via WebSocket on local network:

```
Mobile → Desktop: JSON messages over ws://[IP]:8765
Authentication: Token-based (hardware-bound)
Encryption: TLS optional (local network)
```

---

## System Requirements

### Desktop Application

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| OS | Windows 10 (1903+) | Windows 11 |
| CPU | 2 cores, 2 GHz | 4+ cores |
| RAM | 4 GB | 8+ GB |
| Storage | 200 MB | 500 MB |
| Network | LAN | Wi-Fi 5+ |
| Runtime | WebView2 | WebView2 |

### Mobile Application

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| OS | Android 8.0 (API 26) | Android 12+ |
| RAM | 2 GB | 4+ GB |
| Storage | 100 MB | 200 MB |
| Network | Wi-Fi | Wi-Fi 5+ |
| Permissions | Microphone, Internet | All |

---

## Project Structure

### Desktop (Rust/Tauri)

```
src-tauri/
├── src/
│   ├── main.rs              # Entry point, Tauri setup
│   ├── gemini.rs            # AI integration (Gemini API)
│   ├── mobile_api.rs        # WebSocket server for mobile
│   ├── input.rs             # Mouse/keyboard control
│   ├── browser.rs           # URL opening
│   ├── commands.rs          # System commands
│   ├── win32.rs             # Windows API bindings
│   ├── utils.rs             # Utility functions
│   └── registry.rs          # Windows registry access
├── Cargo.toml               # Rust dependencies
├── tauri.conf.json          # Tauri configuration
└── icons/                   # Application icons
```

### Mobile (Kotlin/Compose)

```
app/src/main/java/com/biz/aris/
├── MainActivity.kt          # Entry point
├── ArisApplication.kt       # Hilt application
├── ui/
│   ├── ArisApp.kt           # Root composable
│   ├── MainViewModel.kt     # Main state management
│   ├── screens/
│   │   ├── MainScreen.kt    # Chat interface
│   │   ├── SettingsScreen.kt# Settings UI
│   │   └── SplashScreen.kt  # Launch screen
│   └── theme/               # Design system
├── service/
│   ├── GeminiService.kt     # Gemini AI client
│   ├── GroqService.kt       # Groq AI client
│   ├── VoiceService.kt      # TTS/STT
│   ├── EdgeTtsService.kt    # Microsoft Edge TTS
│   ├── DesktopConnectionService.kt  # WebSocket client
│   └── WakeOnLanService.kt  # WoL packets
└── data/
    ├── model/               # Data classes
    └── repository/          # Data persistence
```

---

## Build Instructions

### Prerequisites

**Desktop:**
- Rust 1.70+ (rustup.rs)
- Node.js 18+ (for Tauri CLI)
- Visual Studio Build Tools 2019+
- WebView2 Runtime

**Mobile:**
- Android Studio Hedgehog+
- JDK 17+
- Android SDK 34

### Desktop Build

```bash
# Development
cd src-tauri
cargo build

# Release
cargo build --release

# Create installer
cd installer
.\build-installer.ps1 -Release
```

### Mobile Build

```bash
# Debug APK
cd "Aris mobil"
.\gradlew.bat assembleDebug

# Release APK (requires signing)
.\gradlew.bat assembleRelease
```

---

## API Reference

### WebSocket Protocol

All messages are JSON with `type` field discriminator.

#### Authentication

```json
// Client → Server
{
  "type": "auth",
  "token": "XXXX-XXXX-XXXX-XXXX",
  "device_id": "unique_device_id",
  "device_name": "Phone Model"
}

// Server → Client
{
  "type": "auth_response",
  "success": true,
  "message": "Authentication successful",
  "session_id": "session_123456789"
}
```

#### Chat

```json
// Client → Server
{
  "type": "chat",
  "id": "uuid",
  "text": "User message"
}

// Server → Client
{
  "type": "chat_response",
  "id": "uuid",
  "text": "AI response",
  "is_complete": true
}
```

#### Commands

```json
// Client → Server
{
  "type": "command",
  "id": "uuid",
  "action": "set_volume",
  "params": { "level": 50 }
}

// Server → Client
{
  "type": "response",
  "id": "uuid",
  "status": "ok",
  "data": { "volume": 50 }
}
```

#### Available Actions

| Action | Params | Description |
|--------|--------|-------------|
| `set_volume` | `level: 0-100` | Set system volume |
| `open_url` | `url: string` | Open URL in browser |
| `screenshot` | none | Take screenshot |
| `lock` | none | Lock workstation |
| `sleep` | none | Sleep computer |
| `shutdown` | none | Shutdown computer |
| `restart` | none | Restart computer |
| `mouse_move` | `x, y, smooth` | Move mouse |
| `mouse_click` | `button, double` | Click mouse |
| `keyboard_type` | `text` | Type text |
| `keyboard_tap` | `key, modifiers` | Press key |

---

## Third-Party Dependencies

### Desktop (Rust)

| Crate | Version | License | Purpose |
|-------|---------|---------|---------|
| tauri | 1.8 | MIT/Apache-2.0 | Desktop framework |
| tokio | 1.49 | MIT | Async runtime |
| tokio-tungstenite | 0.21 | MIT | WebSocket |
| serde | 1.0 | MIT/Apache-2.0 | Serialization |
| reqwest | 0.11 | MIT/Apache-2.0 | HTTP client |
| enigo | 0.2 | MIT | Input simulation |
| screenshots | 0.8 | MIT | Screen capture |

### Mobile (Kotlin)

| Library | Version | License | Purpose |
|---------|---------|---------|---------|
| Jetpack Compose | 2024.04 | Apache-2.0 | UI framework |
| Hilt | 2.51.1 | Apache-2.0 | DI |
| OkHttp | 4.12 | Apache-2.0 | HTTP/WebSocket |
| Generative AI | 0.9 | Apache-2.0 | Gemini SDK |
| DataStore | 1.1 | Apache-2.0 | Persistence |

### External APIs

| API | License | Notes |
|-----|---------|-------|
| Google Gemini | Terms of Service | Requires API key |
| Groq | Terms of Service | Free tier available |
| Edge TTS | Microsoft Services Agreement | Free, unofficial |

**Important:** Users must obtain their own API keys and comply with respective terms of service.

---

## Security Model

### Authentication

1. **Token Generation**: Hardware-bound token using:
   - Windows Machine GUID (wmic csproduct get UUID)
   - SHA-256 hash with random salt
   - Stored in `%APPDATA%\aris-desktop\mobile_token.txt`

2. **Token Format**: `XXXX-XXXX-XXXX-XXXX` (alphanumeric, no ambiguous chars)

3. **Session**: Created after successful auth, expires on disconnect

### Network Security

- Local network only (no internet exposure by default)
- WebSocket on port 8765 (configurable)
- Optional TLS for encrypted connections
- No data sent to external servers (except AI APIs)

### Data Privacy

- Voice processed locally (on-device STT)
- Chat history stored locally
- API keys encrypted in storage
- No telemetry or analytics

---

## Deployment Guide

### Windows Deployment

1. **Build Release**
   ```bash
   cargo build --release
   ```

2. **Create Installer**
   - Install Inno Setup 6
   - Run `build-installer.ps1 -Release`
   - Distribute `ARIS-Setup-x.x.x.exe`

3. **Silent Install**
   ```bash
   ARIS-Setup-1.2.0.exe /SILENT /DIR="C:\Program Files\ARIS"
   ```

### Android Deployment

1. **Sign APK**
   - Create keystore
   - Configure `signingConfigs` in `build.gradle.kts`

2. **Build Release**
   ```bash
   .\gradlew.bat assembleRelease
   ```

3. **Distribute**
   - APK: Direct installation
   - Play Store: Follow Google guidelines

---

## Maintenance & Updates

### Version Management

- Semantic versioning: MAJOR.MINOR.PATCH
- Desktop and Mobile versions should match for compatibility

### Update Process

1. **Desktop**: Download new installer, run update
2. **Mobile**: Download new APK, install over existing

### Compatibility Matrix

| Desktop | Mobile | Status |
|---------|--------|--------|
| 1.2.x | 1.2.x | ✅ Compatible |
| 1.2.x | 1.1.x | ⚠️ Limited |
| 1.1.x | 1.2.x | ❌ Incompatible |

### Known Issues

1. WebView2 must be installed separately on some Windows versions
2. Android 8-9 may have TTS quality issues
3. Large chat history may slow down sync

### Support Channels

- GitHub Issues: Bug reports, feature requests
- Documentation: This file + README.md
- API Reference: OpenAPI spec (planned)

---

## License Notes

This software uses third-party components with various licenses:
- All Rust crates: MIT or Apache-2.0 (permissive)
- All Android libraries: Apache-2.0 (permissive)
- External APIs: Require user agreement to respective ToS

The buyer/licensee is responsible for:
- Obtaining API keys
- Complying with third-party ToS
- Rebranding if desired
- Ongoing maintenance

---

*Document Version: 1.2.0*
*Last Updated: January 2026*
