# A.R.I.S. Dependencies

Complete list of all external dependencies with their licenses and purposes.

---

## Desktop Application

### Frontend (Node.js/React)

#### Production Dependencies

| Package | Version | License | Purpose |
|---------|---------|---------|---------|
| `@google/genai` | ^1.30.0 | Apache-2.0 | Google Gemini AI SDK |
| `@tauri-apps/api` | ^1.6.0 | MIT/Apache-2.0 | Tauri frontend API |
| `fft-js` | ^0.0.12 | MIT | Fast Fourier Transform for audio visualization |
| `i18next` | ^23.0.0 | MIT | Internationalization framework |
| `idb` | ^8.0.3 | ISC | IndexedDB wrapper for local storage |
| `lucide-react` | ^0.562.0 | ISC | Icon library |
| `motion` | ^12.23.25 | MIT | Animation library (Framer Motion) |
| `react` | ^19.2.0 | MIT | UI library |
| `react-dom` | ^19.2.0 | MIT | React DOM renderer |

#### Development Dependencies

| Package | Version | License | Purpose |
|---------|---------|---------|---------|
| `@tailwindcss/postcss` | ^4.1.18 | MIT | Tailwind CSS PostCSS plugin |
| `@tauri-apps/cli` | ^1.6.3 | MIT/Apache-2.0 | Tauri CLI |
| `@testing-library/jest-dom` | ^6.0.0 | MIT | Testing matchers |
| `@testing-library/react` | ^16.0.0 | MIT | React testing utilities |
| `@types/node` | ^25.0.8 | MIT | Node.js TypeScript types |
| `@vitejs/plugin-react` | ^5.0.0 | MIT | Vite React plugin |
| `@vitest/coverage-v8` | ^2.0.0 | MIT | Test coverage |
| `@xenova/transformers` | ^2.17.2 | Apache-2.0 | ML embeddings (optional) |
| `autoprefixer` | ^10.4.22 | MIT | CSS autoprefixer |
| `jsdom` | ^24.0.0 | MIT | DOM implementation for tests |
| `postcss` | ^8.4.49 | MIT | CSS processor |
| `react-i18next` | ^15.0.0 | MIT | React i18n bindings |
| `tailwindcss` | ^4.1.17 | MIT | Utility-first CSS framework |
| `typescript` | ~5.8.2 | Apache-2.0 | TypeScript compiler |
| `vite` | ^6.2.0 | MIT | Build tool |
| `vitest` | ^2.0.0 | MIT | Test runner |

---

### Backend (Rust/Tauri)

#### Core Dependencies

| Crate | Version | License | Purpose |
|-------|---------|---------|---------|
| `tauri` | 1.8 | MIT/Apache-2.0 | Desktop app framework |
| `tauri-build` | 1.5 | MIT/Apache-2.0 | Build scripts |
| `tokio` | 1.0 | MIT | Async runtime |
| `serde` | 1.0 | MIT/Apache-2.0 | Serialization |
| `serde_json` | 1.0 | MIT/Apache-2.0 | JSON handling |

#### System Integration

| Crate | Version | License | Purpose |
|-------|---------|---------|---------|
| `sysinfo` | 0.30 | MIT | CPU/RAM/process info |
| `dirs` | 5.0 | MIT/Apache-2.0 | User directories |
| `auto-launch` | 0.5 | MIT | Autostart functionality |
| `screenshots` | 0.8 | MIT | Screen capture |
| `enigo` | 0.2 | MIT | Keyboard/mouse simulation |
| `arboard` | 3.4 | MIT/Apache-2.0 | Clipboard access |
| `image` | 0.25 | MIT | Image processing |

#### Networking

| Crate | Version | License | Purpose |
|-------|---------|---------|---------|
| `reqwest` | 0.11 | MIT/Apache-2.0 | HTTP client (Gemini API) |
| `tokio-tungstenite` | 0.21 | MIT | WebSocket server |
| `futures-util` | 0.3 | MIT/Apache-2.0 | Async utilities |
| `hostname` | 0.3 | MIT | Get system hostname |

#### Utilities

| Crate | Version | License | Purpose |
|-------|---------|---------|---------|
| `chrono` | 0.4 | MIT/Apache-2.0 | Date/time handling |
| `base64` | 0.22 | MIT/Apache-2.0 | Base64 encoding |
| `once_cell` | 1.19 | MIT/Apache-2.0 | Lazy statics |
| `rusqlite` | 0.31 | MIT | SQLite database |
| `walkdir` | 2.5 | MIT | Directory traversal |
| `regex` | 1.10 | MIT/Apache-2.0 | Regular expressions |
| `which` | 6.0 | MIT | Find executables |
| `glob` | 0.3 | MIT/Apache-2.0 | File globbing |
| `anyhow` | 1.0 | MIT/Apache-2.0 | Error handling |
| `thiserror` | 1.0 | MIT/Apache-2.0 | Custom errors |
| `log` | 0.4 | MIT/Apache-2.0 | Logging facade |
| `pretty_env_logger` | 0.5 | MIT/Apache-2.0 | Pretty logging |
| `lazy_static` | 1.5 | MIT/Apache-2.0 | Lazy initialization |

#### Windows-Specific

| Crate | Version | License | Purpose |
|-------|---------|---------|---------|
| `windows` | 0.58 | MIT/Apache-2.0 | Windows API bindings |
| `winreg` | 0.52 | MIT | Windows Registry access |

**Windows API Features Used:**
- `Win32_Foundation` — Basic Windows types
- `Win32_UI_WindowsAndMessaging` — Window management
- `Win32_System_Threading` — Process/thread management
- `Win32_System_ProcessStatus` — Process info
- `Win32_System_Power` — Power management (sleep)
- `Win32_System_Shutdown` — System shutdown/restart
- `Win32_Media_Audio` — Volume control
- `Win32_Graphics_Gdi` — Graphics
- `Win32_Security` — Security context

---

## Mobile Application (Android)

### Core Dependencies

| Library | Version | License | Purpose |
|---------|---------|---------|---------|
| `androidx.core:core-ktx` | 1.12+ | Apache-2.0 | Kotlin extensions for Android |
| `androidx.lifecycle:lifecycle-*` | 2.7+ | Apache-2.0 | Lifecycle-aware components |
| `androidx.activity:activity-compose` | 1.8+ | Apache-2.0 | Compose activity support |
| `androidx.compose.ui:ui` | 1.6+ | Apache-2.0 | Jetpack Compose UI |
| `androidx.compose.material3:material3` | 1.2+ | Apache-2.0 | Material 3 components |

### Networking

| Library | Version | License | Purpose |
|---------|---------|---------|---------|
| `com.squareup.okhttp3:okhttp` | 4.12+ | Apache-2.0 | HTTP & WebSocket client |
| `org.jetbrains.kotlinx:kotlinx-serialization-json` | 1.6+ | Apache-2.0 | JSON serialization |

### Dependency Injection

| Library | Version | License | Purpose |
|---------|---------|---------|---------|
| `com.google.dagger:hilt-android` | 2.50+ | Apache-2.0 | Dependency injection |

### Speech & AI

| Library | Version | License | Purpose |
|---------|---------|---------|---------|
| Android Speech API | Built-in | Apache-2.0 | Voice recognition |
| Google Gemini SDK | Latest | Apache-2.0 | AI chat (autonomous mode) |
| Edge TTS (custom) | N/A | MIT | Text-to-speech |

### Storage

| Library | Version | License | Purpose |
|---------|---------|---------|---------|
| `androidx.datastore:datastore-preferences` | 1.0+ | Apache-2.0 | Preferences storage |

---

## External Services

### Google Gemini API

- **Provider:** Google
- **Terms:** https://ai.google.dev/terms
- **Pricing:** Free tier available, paid for higher usage
- **Data Usage:** Text sent to Google for AI processing
- **Required:** Yes (core AI functionality)

### Edge TTS

- **Provider:** Microsoft (unofficial SDK usage)
- **Terms:** Part of Azure Cognitive Services ToS
- **Pricing:** Free (using edge runtime)
- **Data Usage:** Text sent to Microsoft for TTS
- **Required:** No (can use local TTS fallback)

### Web Speech API

- **Provider:** Browser (Chrome/Edge)
- **Terms:** Browser vendor terms
- **Pricing:** Free
- **Data Usage:** Audio may be sent to cloud for recognition
- **Required:** Yes (for voice commands)

---

## License Summary

| License Type | Count | Permissive |
|--------------|-------|------------|
| MIT | 45+ | ✅ Yes |
| Apache-2.0 | 20+ | ✅ Yes |
| MIT/Apache-2.0 (dual) | 15+ | ✅ Yes |
| ISC | 3 | ✅ Yes |

**All dependencies use permissive open-source licenses compatible with commercial use and sale.**

---

## Obtaining API Keys

### Google Gemini API Key

1. Go to https://aistudio.google.com/app/apikey
2. Sign in with Google account
3. Click "Create API Key"
4. Select a Google Cloud project (or create new)
5. Copy the generated key

**Free Tier Limits:**
- 60 requests per minute
- 15 requests per minute for Gemini 1.5 Pro
- No credit card required

### Groq API Key (Optional, for mobile)

1. Go to https://console.groq.com/
2. Sign up / Sign in
3. Navigate to API Keys
4. Generate new key

**Free Tier Limits:**
- 14,400 requests/day
- 30 requests/minute

---

## Security Considerations

### Dependencies with Native Code

These dependencies include native binaries:
- `tauri` — Bundled Rust runtime
- `rusqlite` — Bundled SQLite
- `windows` — Windows API bindings
- `screenshots` — Screen capture (requires permissions)
- `enigo` — Input simulation (requires permissions)

### Network Access

Dependencies with network capabilities:
- `reqwest` — HTTP client
- `tokio-tungstenite` — WebSocket
- `@google/genai` — Gemini API

All network calls are:
1. To `localhost` (WebSocket server)
2. To `generativelanguage.googleapis.com` (Gemini API)

---

## Updating Dependencies

### Frontend (npm)

```bash
# Check for updates
npm outdated

# Update all (respecting semver)
npm update

# Update to latest (may break)
npm update --latest

# Update specific package
npm install package@latest
```

### Backend (Cargo)

```bash
# Check for updates
cargo outdated

# Update Cargo.lock
cargo update

# Update specific crate
cargo update -p serde
```

### Security Audits

```bash
# npm audit
npm audit

# Fix vulnerabilities
npm audit fix

# Rust audit
cargo audit
```

---

## Vendoring Dependencies

For airgapped builds or dependency archival:

### npm

```bash
# Create node_modules tarball
npm pack

# Or use npm-pack-all
npx npm-pack-all
```

### Cargo

```bash
# Vendor all crates
cargo vendor

# Configure .cargo/config.toml
[source.crates-io]
replace-with = "vendored-sources"

[source.vendored-sources]
directory = "vendor"
```

---

## Size Impact

### Desktop (Release Build)

| Component | Size |
|-----------|------|
| Frontend (dist/) | ~2 MB |
| Rust binary | ~15 MB |
| WebView2 Bootstrapper | ~2 MB |
| Installer total | ~25 MB |

### Mobile (Release APK)

| Component | Size |
|-----------|------|
| Kotlin/Android | ~5 MB |
| Compose UI | ~3 MB |
| OkHttp | ~1 MB |
| Total APK | ~15 MB |
