# A.R.I.S. Architecture Documentation

## Overview

A.R.I.S. (Advanced Research & Intelligence System) is a cross-platform AI assistant with:
- **Desktop App** — Tauri-based Windows application (Rust + React/TypeScript)
- **Mobile App** — Native Android application (Kotlin + Jetpack Compose)

Both communicate via **WebSocket** over local network (LAN).

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER LAYER                               │
├──────────────────────────────────┬──────────────────────────────┤
│         Desktop (Windows)        │       Mobile (Android)        │
│    ┌─────────────────────────┐   │   ┌─────────────────────────┐ │
│    │     React Frontend      │   │   │   Jetpack Compose UI    │ │
│    │    (TypeScript/Vite)    │   │   │      (Kotlin)           │ │
│    └───────────┬─────────────┘   │   └───────────┬─────────────┘ │
│                │                 │               │               │
│    ┌───────────▼─────────────┐   │   ┌───────────▼─────────────┐ │
│    │   Tauri WebView2        │   │   │    Android ViewModel    │ │
│    │   (IPC Bridge)          │   │   │    (Hilt DI)            │ │
│    └───────────┬─────────────┘   │   └───────────┬─────────────┘ │
│                │                 │               │               │
│    ┌───────────▼─────────────┐   │   ┌───────────▼─────────────┐ │
│    │    Rust Backend         │   │   │   WebSocket Client      │ │
│    │    (Tokio async)        │◄──┼───┤   (OkHttp)              │ │
│    └───────────┬─────────────┘   │   └─────────────────────────┘ │
│                │                 │                               │
├────────────────┼─────────────────┴───────────────────────────────┤
│                │            SERVICE LAYER                        │
│    ┌───────────▼─────────────┐                                   │
│    │   WebSocket Server      │ Port 8765                         │
│    │   (mobile_api.rs)       │                                   │
│    └───────────┬─────────────┘                                   │
│                │                                                 │
│    ┌───────────┴───────────────────────────────────┐             │
│    │           Core Services (Rust)                 │             │
│    ├────────────┬──────────────┬──────────────────┤             │
│    │ commands   │ gemini       │ input            │             │
│    │ browser    │ file_ops     │ system           │             │
│    │ win32      │ memory       │ clipboard        │             │
│    └────────────┴──────────────┴──────────────────┘             │
│                                                                  │
├──────────────────────────────────────────────────────────────────┤
│                        EXTERNAL SERVICES                         │
│    ┌────────────────┐  ┌────────────────┐  ┌──────────────────┐ │
│    │  Google Gemini │  │  Edge TTS      │  │  Web Speech API  │ │
│    │  (AI/Chat)     │  │  (Voice Synth) │  │  (Voice Recog)   │ │
│    └────────────────┘  └────────────────┘  └──────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```

---

## Desktop Application (Tauri)

### Directory Structure

```
├── src-tauri/
│   ├── src/
│   │   ├── main.rs           # Entry point, window/tray management
│   │   ├── commands.rs       # Tauri IPC commands
│   │   ├── mobile_api.rs     # WebSocket server for mobile
│   │   ├── gemini.rs         # Google Gemini AI integration
│   │   ├── browser.rs        # Browser/URL operations
│   │   ├── input.rs          # Keyboard/mouse control
│   │   ├── win32.rs          # Windows-specific APIs
│   │   ├── system.rs         # System info (CPU, RAM)
│   │   ├── file_ops.rs       # File system operations
│   │   ├── memory.rs         # Knowledge base storage
│   │   ├── clipboard.rs      # Clipboard operations
│   │   ├── config.rs         # Configuration management
│   │   ├── autostart.rs      # Windows autostart registry
│   │   └── window.rs         # Window management
│   ├── tauri.conf.json       # Tauri configuration
│   └── Cargo.toml            # Rust dependencies
│
├── components/               # React UI components
│   ├── CoreVisual.tsx        # Animated core visualization
│   ├── ChatPanel.tsx         # Chat interface
│   ├── SettingsPanel.tsx     # Settings UI
│   ├── SystemHUD.tsx         # System status display
│   ├── FrequencyVisualizer.tsx
│   └── ...
│
├── services/                 # TypeScript services
│   ├── geminiService.ts      # AI chat service
│   ├── commandHandler.ts     # Voice command parsing
│   ├── settingsService.ts    # App settings management
│   ├── conversationMemory.ts # Chat history
│   ├── unifiedKnowledgeBase.ts
│   └── ...
│
├── hooks/                    # React hooks
│   ├── useAppInitialization.ts
│   ├── useArisConnection.ts
│   ├── useMiniMode.ts
│   └── ...
│
└── App.tsx                   # Main React component
```

### Rust Backend Modules

| Module | Description |
|--------|-------------|
| `main.rs` | App entry, system tray, window events, global shortcuts |
| `commands.rs` | Tauri IPC commands exposed to frontend |
| `mobile_api.rs` | WebSocket server (port 8765) for mobile clients |
| `gemini.rs` | Google Gemini API client with streaming support |
| `browser.rs` | Open URLs in default browser |
| `input.rs` | Keyboard/mouse simulation via Windows API |
| `win32.rs` | Low-level Windows APIs (lock, sleep, volume) |
| `system.rs` | CPU/RAM usage, process list |
| `file_ops.rs` | File read/write with proper encoding |
| `memory.rs` | Persistent knowledge storage (JSON) |
| `config.rs` | API key and settings management |
| `autostart.rs` | Windows registry autostart setup |

### Frontend Architecture

```
App.tsx
├── useAppInitialization()     # API key, settings, boot sequence
├── useArisConnection()        # WebSocket, voice, AI connection
└── Components
    ├── BootScreen             # Startup animation
    ├── CoreVisual             # Animated visualization
    ├── ChatPanel              # Message display
    ├── SystemHUD              # System metrics
    ├── ControlPanel           # Voice/system buttons
    ├── SettingsPanel          # Configuration UI
    └── WindowControls         # Frameless window buttons
```

---

## Mobile Application (Android)

### Architecture Pattern: MVVM + Clean Architecture

```
┌─────────────────────────────────────────────┐
│                 UI Layer                     │
│  (Jetpack Compose Screens & Components)     │
├─────────────────────────────────────────────┤
│              ViewModel Layer                 │
│   (StateFlow, Event handling, UI logic)     │
├─────────────────────────────────────────────┤
│              Domain Layer                    │
│   (Use Cases, Business Logic)               │
├─────────────────────────────────────────────┤
│               Data Layer                     │
│  (Repositories, WebSocket, Local Storage)   │
└─────────────────────────────────────────────┘
```

### Key Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| UI | Jetpack Compose | Declarative UI |
| DI | Hilt | Dependency Injection |
| Network | OkHttp | WebSocket client |
| Voice | Android SpeechRecognizer | Voice input |
| TTS | Edge TTS / Android TTS | Voice output |
| AI | Gemini / Groq API | Autonomous mode |
| Storage | DataStore | Preferences |

---

## WebSocket Communication Protocol

### Connection Flow

```
Mobile                                      Desktop
  │                                            │
  │──────── TCP Connect (port 8765) ──────────►│
  │                                            │
  │◄─────── WebSocket Handshake ──────────────│
  │                                            │
  │──────── Auth { token, device_id } ────────►│
  │                                            │
  │◄─────── AuthResponse { success, session } ─│
  │                                            │
  │◄═══════ Bidirectional Messages ══════════►│
  │                                            │
```

### Message Types

See [API_DOCS.md](API_DOCS.md) for complete protocol specification.

| Type | Direction | Purpose |
|------|-----------|---------|
| `auth` | M → D | Authentication with token |
| `auth_response` | D → M | Auth result |
| `command` | M → D | Execute PC command |
| `response` | D → M | Command result |
| `chat` | M → D | AI chat message |
| `chat_response` | D → M | AI response |
| `sync_request` | M → D | Sync knowledge/history |
| `sync_response` | D → M | Synced data |
| `tts_request` | M → D | Text-to-speech |
| `event` | D → M | Desktop events |
| `ping/pong` | Both | Keepalive |

---

## Data Flow

### Voice Command Flow (Mobile → Desktop)

```
User speaks
    │
    ▼
┌──────────────────┐
│ Android Speech   │
│ Recognizer       │
└────────┬─────────┘
         │ text
         ▼
┌──────────────────┐
│ WebSocket Send   │
│ { type: "chat" } │
└────────┬─────────┘
         │
    ═════╪═════ Network ═════
         │
         ▼
┌──────────────────┐
│ Desktop WS       │
│ Server           │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Command          │ ───► Execute (open URL, volume, etc.)
│ Detection        │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Gemini AI        │ ───► Generate response
│ Processing       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ WebSocket Send   │
│ { type: "chat_  │
│   response" }   │
└────────┬─────────┘
         │
    ═════╪═════ Network ═════
         │
         ▼
┌──────────────────┐
│ Mobile receives  │
│ & displays       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Edge TTS         │
│ Speaks response  │
└──────────────────┘
```

---

## Security Model

### Authentication

1. **Token Generation** — Unique token generated on first Desktop launch
   - Based on Windows Machine GUID (hardware-bound)
   - Stored in `%APPDATA%\aris-desktop\mobile_token.txt`
   - Format: `XXXX-XXXX-XXXX-XXXX` (16 chars)

2. **Token Verification** — Every WebSocket connection must authenticate
   - First message must be `auth` type with valid token
   - Unauthenticated messages receive `UNAUTHORIZED` error

3. **Session Management**
   - Session ID generated on successful auth
   - Used for tracking connected devices
   - Automatic cleanup on disconnect

### Network Security

- **Local Network Only** — WebSocket binds to `0.0.0.0:8765`
- **No Internet Exposure** — No cloud relay, no port forwarding
- **Same WiFi Requirement** — Desktop and Mobile must be on same LAN

### Data Privacy

- **Local Processing** — Voice recognition via Web Speech API (browser)
- **No Voice Storage** — Audio not persisted
- **Gemini API** — Only chat text sent to Google (per their ToS)

---

## Configuration Files

| File | Location | Purpose |
|------|----------|---------|
| `mobile_token.txt` | `%APPDATA%\aris-desktop\` | Mobile auth token |
| `settings.json` | `%APPDATA%\aris-desktop\` | App settings |
| `config.json` | `%APPDATA%\aris-desktop\` | API key storage |
| `knowledge.json` | `%APPDATA%\aris-desktop\` | Learned facts |
| `aris_YYYYMMDD.log` | `%APPDATA%\aris-desktop\logs\` | Daily logs |

---

## Performance Considerations

### Desktop

- **Rust Backend** — Zero-cost abstractions, no GC pauses
- **WebView2** — Hardware-accelerated rendering
- **Async I/O** — Tokio runtime for non-blocking operations
- **Lazy Loading** — Services initialized on demand

### Mobile

- **Compose** — Efficient recomposition
- **Coroutines** — Structured concurrency
- **Connection Pooling** — OkHttp connection reuse
- **Background Limits** — Respect Android Doze mode

### UI Performance

- **CoreVisual Quality Settings** — Low/Medium/High
- **Animation Throttling** — Disabled when minimized
- **Debounced Updates** — Metrics update every 2s (configurable)

---

## Extensibility

### Adding New Commands

1. **Desktop (Rust)** — Add to `detect_and_execute_command()` in `mobile_api.rs`
2. **Frontend (TS)** — Add to command handlers in `services/commandHandler.ts`

### Adding New UI Components

1. Create component in `components/`
2. Import in `App.tsx`
3. Add to appropriate render section

### Adding New Services

1. Create service in `services/`
2. Initialize in `useAppInitialization.ts`
3. Expose via context if needed globally

---

## Build System

### Desktop

```bash
# Development
npm run tauri dev

# Production build
npm run tauri build
```

Output: `src-tauri/target/release/bundle/msi/ARIS-Setup-*.exe`

### Mobile

```bash
# Debug APK
./gradlew assembleDebug

# Release APK (signed)
./gradlew assembleRelease
```

---

## Dependencies Overview

See [DEPENDENCIES.md](DEPENDENCIES.md) for complete list with licenses.

### Core Desktop Dependencies

- `tauri` — App framework (MIT/Apache-2.0)
- `tokio` — Async runtime (MIT)
- `tokio-tungstenite` — WebSocket (MIT)
- `serde` — Serialization (MIT/Apache-2.0)
- `reqwest` — HTTP client (MIT/Apache-2.0)

### Core Frontend Dependencies

- `react` — UI library (MIT)
- `vite` — Build tool (MIT)
- `tailwindcss` — Styling (MIT)
- `i18next` — Internationalization (MIT)

### Core Mobile Dependencies

- `androidx.*` — Android components (Apache-2.0)
- `okhttp` — HTTP/WebSocket (Apache-2.0)
- `hilt` — DI (Apache-2.0)
- `kotlin-coroutines` — Async (Apache-2.0)
