# Third-Party Components and Licenses

This document lists all third-party components used in this software, their licenses, and usage requirements.

---

## Overview

| Category | Components | License Type |
|----------|------------|--------------|
| Desktop (Rust) | 50+ crates | MIT / Apache-2.0 |
| Mobile (Kotlin) | 20+ libraries | Apache-2.0 |
| External APIs | 3 services | Terms of Service |

All included components use **permissive open-source licenses** that allow commercial use.

---

## Desktop Application (Rust)

### Core Framework

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| Tauri | 1.8.3 | MIT/Apache-2.0 | https://tauri.app |
| Tokio | 1.49.0 | MIT | https://tokio.rs |
| Serde | 1.0.228 | MIT/Apache-2.0 | https://serde.rs |

### Networking

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| tokio-tungstenite | 0.21.0 | MIT | WebSocket implementation |
| reqwest | 0.11.27 | MIT/Apache-2.0 | HTTP client |
| hyper | 0.14.32 | MIT | HTTP library |

### System Integration

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| enigo | 0.2.1 | MIT | Input simulation |
| screenshots | 0.8.10 | MIT | Screen capture |
| arboard | 3.6.1 | MIT/Apache-2.0 | Clipboard |
| windows | 0.61.3 | MIT/Apache-2.0 | Windows API |

### Utilities

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| chrono | 0.4.42 | MIT/Apache-2.0 | Date/time |
| base64 | 0.22.1 | MIT/Apache-2.0 | Encoding |
| sysinfo | 0.30.13 | MIT | System info |
| hostname | 0.3.1 | MIT | Hostname |

---

## Mobile Application (Kotlin/Android)

### UI Framework

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| Jetpack Compose | BOM 2024.04.01 | Apache-2.0 | https://developer.android.com/compose |
| Material 3 | 1.2.1 | Apache-2.0 | Material Design |
| Navigation Compose | 2.7.7 | Apache-2.0 | Navigation |

### Dependency Injection

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| Hilt | 2.51.1 | Apache-2.0 | https://dagger.dev/hilt |
| Hilt Navigation | 1.2.0 | Apache-2.0 | Compose integration |

### Networking

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| OkHttp | 4.12.0 | Apache-2.0 | https://square.github.io/okhttp |
| Kotlinx Serialization | 1.6.3 | Apache-2.0 | JSON parsing |

### AI Integration

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| Google Generative AI | 0.9.0 | Apache-2.0 | Gemini SDK |

### Data Persistence

| Component | Version | License | URL |
|-----------|---------|---------|-----|
| DataStore | 1.1.1 | Apache-2.0 | Preferences |
| Room | 2.6.1 | Apache-2.0 | SQLite ORM |

---

## External APIs

### Google Gemini API

**Type:** Cloud AI Service  
**License:** Google API Terms of Service  
**URL:** https://ai.google.dev  

**Requirements:**
- Users must obtain their own API key
- Usage subject to Google's Terms of Service
- Rate limits and quotas apply
- Commercial use allowed

**Pricing:** Free tier available, pay-per-use for higher volumes

---

### Groq API

**Type:** Cloud AI Service  
**License:** Groq Terms of Service  
**URL:** https://groq.com  

**Requirements:**
- Users must create account and obtain API key
- Subject to Groq's Terms of Service
- Free tier with generous limits

**Pricing:** Free tier available

---

### Microsoft Edge TTS (Unofficial)

**Type:** Text-to-Speech Service  
**License:** Microsoft Services Agreement (unofficial access)  
**URL:** https://azure.microsoft.com/services/cognitive-services/text-to-speech/  

**Important Notes:**
- This implementation uses an unofficial endpoint
- For production use, consider official Azure Cognitive Services
- Official API provides guaranteed SLA and support

**Alternative:** Android's built-in TTS (offline, free)

---

## License Texts

### MIT License (used by most Rust crates)

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Apache License 2.0 (used by Android libraries)

```
Apache License
Version 2.0, January 2004
http://www.apache.org/licenses/

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

---

## Compliance Checklist

For commercial distribution, ensure:

- [ ] All MIT/Apache licenses preserved in distribution
- [ ] API keys obtained by end users
- [ ] Terms of Service accepted for external APIs
- [ ] Edge TTS replaced with official Azure API (recommended)
- [ ] Proper attribution in "About" section

---

## Rebranding Considerations

If rebranding the software:

1. **Safe to change:**
   - Application name
   - Visual design/icons
   - UI text and branding

2. **Must preserve:**
   - Third-party license notices
   - Open-source attributions
   - API usage compliance

3. **Recommended:**
   - Replace unofficial Edge TTS with Azure TTS
   - Register your own trademark
   - Update all documentation

---

*Last Updated: January 2026*
