# A.R.I.S. Build Instructions

This document describes how to build A.R.I.S. Desktop and Mobile applications from source.

---

## Prerequisites

### Desktop (Windows)

| Requirement | Version | Download |
|-------------|---------|----------|
| Node.js | 18+ | https://nodejs.org/ |
| Rust | 1.70+ | https://rustup.rs/ |
| Visual Studio Build Tools | 2019+ | https://visualstudio.microsoft.com/downloads/ |
| WebView2 Runtime | Latest | Auto-bundled in installer |

### Mobile (Android)

| Requirement | Version | Download |
|-------------|---------|----------|
| Android Studio | Arctic Fox+ | https://developer.android.com/studio |
| JDK | 17+ | https://adoptium.net/ |
| Android SDK | API 26+ (Android 8.0) | Via Android Studio |
| Kotlin | 1.9+ | Via Android Studio |

---

## Desktop Build

### 1. Clone & Install Dependencies

```bash
# Clone repository
git clone <repo-url>
cd aris-desktop

# Install Node.js dependencies
npm install

# Rust dependencies are handled automatically by Cargo
```

### 2. Development Mode

```bash
# Start development server (hot reload)
npm run dev
```

This will:
1. Build the React frontend with Vite
2. Start Tauri in development mode
3. Open the application window
4. Enable DevTools (F12)

### 3. Production Build

```bash
# Build for Windows
npm run build:tauri
```

Output: `src-tauri/target/release/bundle/`
- `msi/ARIS-Setup-1.2.0.exe` — Installer
- `nsis/ARIS_1.2.0_x64-setup.exe` — NSIS installer

### 4. Build Commands Reference

| Command | Description |
|---------|-------------|
| `npm run dev` | Development mode with hot reload |
| `npm run dev:frontend` | Frontend only (Vite dev server) |
| `npm run build` | Build frontend only |
| `npm run build:tauri` | Full production build |
| `npm run build:win` | Build for Windows x64 |
| `npm run test` | Run tests (watch mode) |
| `npm run test:run` | Run tests once |

### 5. Troubleshooting

#### Rust compilation errors

```bash
# Update Rust toolchain
rustup update

# Clean and rebuild
cd src-tauri
cargo clean
cd ..
npm run build:tauri
```

#### WebView2 missing

The installer bundles WebView2 bootstrapper. For development:
- Windows 10/11 usually has it pre-installed
- Download manually: https://developer.microsoft.com/en-us/microsoft-edge/webview2/

#### Windows SDK errors

Install Windows 10/11 SDK via Visual Studio Installer:
1. Open Visual Studio Installer
2. Modify → Individual Components
3. Check "Windows 10 SDK (10.0.19041.0)" or newer

---

## Mobile Build (Android)

### 1. Project Setup

The mobile app is a separate project. Obtain `aris-mobile` repository.

```bash
cd aris-mobile

# Sync Gradle dependencies
./gradlew sync
```

### 2. Configure Build

Edit `local.properties`:
```properties
sdk.dir=/path/to/Android/Sdk
```

### 3. Debug Build

```bash
# Build debug APK
./gradlew assembleDebug
```

Output: `app/build/outputs/apk/debug/app-debug.apk`

### 4. Release Build

#### Generate Keystore (first time only)

```bash
keytool -genkey -v -keystore aris-release.keystore \
  -alias aris -keyalg RSA -keysize 2048 -validity 10000
```

#### Configure Signing

Create `keystore.properties`:
```properties
storePassword=your_password
keyPassword=your_password
keyAlias=aris
storeFile=../aris-release.keystore
```

#### Build Release APK

```bash
./gradlew assembleRelease
```

Output: `app/build/outputs/apk/release/app-release.apk`

### 5. Build Commands Reference

| Command | Description |
|---------|-------------|
| `./gradlew assembleDebug` | Debug APK |
| `./gradlew assembleRelease` | Release APK (signed) |
| `./gradlew installDebug` | Install on connected device |
| `./gradlew clean` | Clean build artifacts |
| `./gradlew test` | Run unit tests |
| `./gradlew connectedAndroidTest` | Run instrumented tests |

---

## Configuration

### Gemini API Key

The application requires a Google Gemini API key.

#### Get API Key

1. Go to https://aistudio.google.com/app/apikey
2. Click "Create API Key"
3. Copy the key

#### Configure in App

**Desktop:**
1. Launch application
2. Open Settings (gear icon)
3. Paste API key in "Gemini API Key" field
4. Click Save

Key is stored in: `%APPDATA%\aris-desktop\config.json`

**Mobile:**
1. Open Settings
2. Enter API key (for Autonomous mode)

#### Environment Variable (optional)

```bash
# Windows
set GEMINI_API_KEY=your_api_key_here

# PowerShell
$env:GEMINI_API_KEY = "your_api_key_here"
```

---

## Build Artifacts

### Desktop

```
src-tauri/target/release/
├── aris-desktop.exe          # Standalone executable
└── bundle/
    ├── msi/
    │   └── ARIS-Setup-1.2.0.msi
    └── nsis/
        └── ARIS_1.2.0_x64-setup.exe
```

### Mobile

```
app/build/outputs/
├── apk/
│   ├── debug/
│   │   └── app-debug.apk
│   └── release/
│       └── app-release.apk
└── bundle/
    └── release/
        └── app-release.aab    # For Play Store
```

---

## CI/CD (Optional)

### GitHub Actions Example

```yaml
name: Build

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-desktop:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          
      - name: Setup Rust
        uses: dtolnay/rust-action@stable
        
      - name: Install dependencies
        run: npm install
        
      - name: Build
        run: npm run build:tauri
        
      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: aris-desktop
          path: src-tauri/target/release/bundle/

  build-mobile:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup JDK
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
          
      - name: Build APK
        run: ./gradlew assembleRelease
        
      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: aris-mobile
          path: app/build/outputs/apk/release/
```

---

## Customization

### Changing App Name

1. **Tauri config:** `src-tauri/tauri.conf.json`
   ```json
   {
     "package": {
       "productName": "Your App Name"
     }
   }
   ```

2. **Package.json:** Update `name` and `description`

3. **Cargo.toml:** Update `name` and `description`

### Changing Icons

Replace icons in:
- `src-tauri/icons/` (all sizes)
- `assets/icon.png`

Generate icon set:
```bash
# Using tauri CLI
cargo tauri icon path/to/icon.png
```

### Changing Port

WebSocket port (default: 8765) is defined in:
- `src-tauri/src/main.rs` — `start_server(8765)`
- Mobile app settings

---

## Testing

### Desktop Tests

```bash
# Run all tests
npm run test

# Run once (CI mode)
npm run test:run

# With coverage
npm run test:coverage
```

Tests are located in `__tests__/` directory.

### Mobile Tests

```bash
# Unit tests
./gradlew test

# Instrumented tests (requires device/emulator)
./gradlew connectedAndroidTest
```

---

## Release Checklist

Before releasing:

- [ ] Update version in `package.json`
- [ ] Update version in `src-tauri/Cargo.toml`
- [ ] Update version in `src-tauri/tauri.conf.json`
- [ ] Run all tests: `npm run test:run`
- [ ] Build production: `npm run build:tauri`
- [ ] Test installer on clean Windows VM
- [ ] Verify API key flow works
- [ ] Check mobile connection
- [ ] Update CHANGELOG.md
- [ ] Tag release in Git: `git tag v1.2.0`
