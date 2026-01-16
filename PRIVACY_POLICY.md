# Privacy Policy

## A.R.I.S. — Advanced Research & Intelligence System

**Effective Date:** January 2026  
**Last Updated:** January 2026

---

## Overview

A.R.I.S. is designed with privacy in mind. This policy explains what data is collected, how it's used, and your rights regarding your data.

---

## Data Collection

### Data Collected Locally

| Data Type | Storage Location | Purpose |
|-----------|------------------|---------|
| Conversation history | Local SQLite database | Context for AI responses |
| Knowledge base | Local SQLite database | Personalized assistance |
| Settings/preferences | Local JSON files | Application configuration |
| Logs | Local log files | Debugging and diagnostics |

### Data Sent to Third Parties

| Data Type | Recipient | Purpose |
|-----------|-----------|---------|
| Voice audio | Google/Microsoft (via browser) | Speech recognition |
| Text queries | Google Gemini API | AI response generation |
| TTS text | Microsoft Edge TTS | Speech synthesis |

---

## Data Storage

### Local Storage

All user data is stored locally on your device:

- **Windows:** `%APPDATA%\aris-desktop\`
- **Conversation data:** SQLite database
- **Settings:** `settings.json`
- **Logs:** `logs/` directory

### No Cloud Storage

A.R.I.S. does NOT:
- Store your data on remote servers
- Create user accounts
- Require registration
- Sync data to the cloud

---

## Third-Party Services

### Google Gemini API

When you send a message to A.R.I.S., the text is sent to Google Gemini API for processing.

**Data sent:**
- Your text message
- Conversation context (recent messages)

**Google's privacy policy:** https://policies.google.com/privacy

### Web Speech API

Voice recognition uses your browser's Web Speech API.

**Data sent:**
- Audio from your microphone

**Note:** Audio processing may occur on Google or Microsoft servers depending on your browser.

### Microsoft Edge TTS

Text-to-speech uses Microsoft Edge TTS.

**Data sent:**
- Text to be spoken

---

## Data Retention

### Local Data

| Data Type | Retention Period |
|-----------|------------------|
| Conversation history | Until manually deleted |
| Knowledge base | Until manually deleted |
| Logs | 10 files max, auto-rotated |
| Settings | Until manually deleted |

### Third-Party Data

Data sent to third-party services is subject to their respective retention policies:
- Google: https://policies.google.com/technologies/retention
- Microsoft: https://privacy.microsoft.com/

---

## Your Rights

### Access

You can access all your data by navigating to:
- `%APPDATA%\aris-desktop\` (Windows)

### Deletion

You can delete your data by:
1. Deleting the `aris-desktop` folder
2. Using the "Reset Settings" option in the app
3. Uninstalling the application

### Portability

Your data is stored in standard formats (JSON, SQLite) and can be exported manually.

---

## Security Measures

### Local Security

- Data stored in user-specific directories
- No network transmission of local data
- Token-based authentication for mobile connection

### Network Security

- WebSocket communication on local network only
- Authentication required for mobile connections
- No data transmitted over the internet (except to APIs)

---

## Children's Privacy

A.R.I.S. is not intended for use by children under 13. We do not knowingly collect data from children.

---

## Changes to This Policy

We may update this privacy policy from time to time. Changes will be reflected in the "Last Updated" date.

---

## Contact

For privacy-related inquiries:

**Email:** yrekkomar@gmail.com

---

## Summary

| Aspect | Status |
|--------|--------|
| User accounts | ❌ Not required |
| Cloud storage | ❌ None |
| Data selling | ❌ Never |
| Local storage | ✅ On your device |
| Third-party APIs | ✅ Google Gemini, Edge TTS |
| Data deletion | ✅ Full control |

**Your data stays on your device. We don't collect, store, or sell your personal information.**
