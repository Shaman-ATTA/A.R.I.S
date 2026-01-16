# A.R.I.S. WebSocket API Documentation

## Overview

The A.R.I.S. Desktop application runs a WebSocket server on port **8765** that allows mobile clients to connect and control the PC remotely.

**Protocol:** WebSocket (RFC 6455)  
**Port:** 8765 (configurable)  
**Encoding:** JSON over Text frames  

---

## Connection

### Endpoint

```
ws://<desktop-ip>:8765
```

Example: `ws://192.168.1.100:8765`

### Finding Desktop IP

On Windows Desktop, run:
```cmd
ipconfig
```
Look for "IPv4 Address" under your active network adapter.

---

## Authentication

All connections must authenticate before sending other messages.

### Auth Request

```json
{
  "type": "auth",
  "token": "XXXX-XXXX-XXXX-XXXX",
  "device_id": "unique-device-identifier",
  "device_name": "My Phone"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `token` | string | Auth token from Desktop (shown at startup) |
| `device_id` | string | Unique identifier for this device |
| `device_name` | string | Human-readable device name |

### Auth Response

**Success:**
```json
{
  "type": "auth_response",
  "success": true,
  "message": "Authentication successful",
  "session_id": "session_1705123456789"
}
```

**Failure:**
```json
{
  "type": "auth_response",
  "success": false,
  "message": "Invalid token",
  "session_id": null
}
```

---

## Message Types

### 1. Chat / AI Conversation

#### Chat Request (Mobile → Desktop)

```json
{
  "type": "chat",
  "id": "msg-uuid-12345",
  "text": "What's the weather like?"
}
```

#### Chat Response (Desktop → Mobile)

```json
{
  "type": "chat_response",
  "id": "msg-uuid-12345",
  "text": "I don't have real-time weather data, but you can check weather.com.",
  "is_complete": true
}
```

The `is_complete` field indicates if streaming is finished (always `true` currently).

---

### 2. Commands

Execute system commands on the Desktop.

#### Command Request (Mobile → Desktop)

```json
{
  "type": "command",
  "id": "cmd-uuid-12345",
  "action": "screenshot",
  "params": null
}
```

#### Command Response (Desktop → Mobile)

```json
{
  "type": "response",
  "id": "cmd-uuid-12345",
  "status": "ok",
  "data": { "image": "base64...", "format": "png" },
  "error": null
}
```

**Status values:** `"ok"` | `"error"`

---

### Available Commands

| Action | Params | Response Data | Description |
|--------|--------|---------------|-------------|
| `screenshot` | — | `{ image, format }` | Take screenshot (base64 PNG) |
| `open_url` | `{ url }` | `{ opened }` | Open URL in browser |
| `volume` | `{ level: 0-100 }` | `{ volume }` | Set system volume |
| `lock` | — | `{ locked }` | Lock workstation |
| `sleep` | — | `{ sleeping }` | Enter sleep mode |
| `system_info` | — | `{ os, arch, hostname }` | Get system info |
| `keyboard_type` | `{ text }` | `{ typed }` | Type text |
| `mouse_click` | `{ x?, y?, button? }` | `{ clicked }` | Mouse click |
| `execute` | `{ command }` | `{ stdout, stderr, exit_code }` | Run shell command |

---

### 3. Knowledge & Conversation Sync

Synchronize shared data between devices.

#### Sync Request (Mobile → Desktop)

```json
{
  "type": "sync_request",
  "knowledge": [
    {
      "id": "k-1",
      "key": "user_name",
      "value": "Alex",
      "source": "mobile",
      "timestamp": 1705123456,
      "importance": 0.8
    }
  ],
  "conversation": [
    {
      "id": "c-1",
      "text": "Hello ARIS",
      "sender": "user",
      "timestamp": 1705123400
    }
  ]
}
```

#### Sync Response (Desktop → Mobile)

```json
{
  "type": "sync_response",
  "knowledge": [ /* merged knowledge entries */ ],
  "conversation": [ /* merged conversation entries */ ]
}
```

---

### 4. Text-to-Speech

Request desktop to generate speech audio.

#### TTS Request (Mobile → Desktop)

```json
{
  "type": "tts_request",
  "id": "tts-uuid-12345",
  "text": "Hello, how can I help you?"
}
```

#### TTS Response (Desktop → Mobile)

```json
{
  "type": "tts_audio",
  "id": "tts-uuid-12345",
  "audio": "base64-encoded-mp3",
  "format": "mp3"
}
```

**Note:** Currently returns error; mobile should use Edge TTS directly.

---

### 5. Voice Input

Send voice audio for transcription on desktop.

#### Voice Input (Mobile → Desktop)

```json
{
  "type": "voice_input",
  "id": "voice-uuid-12345",
  "audio": "base64-encoded-pcm",
  "format": "pcm_16k"
}
```

#### Transcription Response (Desktop → Mobile)

```json
{
  "type": "voice_transcription",
  "id": "voice-uuid-12345",
  "text": "Open YouTube"
}
```

**Note:** Currently returns error; use mobile speech recognition.

---

### 6. Events

Desktop broadcasts events to all connected clients.

#### Event (Desktop → Mobile)

```json
{
  "type": "event",
  "event_type": "command_executed",
  "data": {
    "command": "screenshot",
    "success": true
  }
}
```

**Common event types:**
- `command_executed`
- `volume_changed`
- `screen_locked`
- `new_message`

---

### 7. Ping/Pong (Keepalive)

#### Ping (Either direction)

```json
{
  "type": "ping",
  "timestamp": 1705123456789
}
```

#### Pong (Response)

```json
{
  "type": "pong",
  "timestamp": 1705123456789
}
```

---

### 8. Error

#### Error Response

```json
{
  "type": "error",
  "code": "UNAUTHORIZED",
  "message": "Authentication required"
}
```

**Error codes:**
| Code | Description |
|------|-------------|
| `PARSE_ERROR` | Invalid JSON or unknown message type |
| `UNAUTHORIZED` | Not authenticated |
| `COMMAND_ERROR` | Command execution failed |
| `TTS_ERROR` | TTS generation failed |
| `VOICE_ERROR` | Voice processing failed |
| `INTERNAL_ERROR` | Server-side error |

---

## Data Structures

### KnowledgeEntry

```typescript
interface KnowledgeEntry {
  id: string;         // Unique identifier
  key: string;        // Knowledge key (e.g., "user_name")
  value: string;      // Knowledge value
  source: string;     // "mobile" | "desktop" | "user"
  timestamp: number;  // Unix timestamp (seconds)
  importance: number; // 0.0 to 1.0
}
```

### ChatEntry

```typescript
interface ChatEntry {
  id: string;         // Unique identifier
  text: string;       // Message content
  sender: string;     // "user" | "aris" | "system"
  timestamp: number;  // Unix timestamp (seconds)
}
```

---

## Natural Language Commands

When sending a `chat` message, the Desktop will attempt to detect and execute PC commands before processing with AI.

### Supported Commands (Russian/English)

| Intent | Example Phrases |
|--------|-----------------|
| Open YouTube | "открой ютуб", "open youtube" |
| Open Google | "открой гугл", "open google" |
| Open VK | "открой вк", "open vk" |
| Open Browser | "открой браузер", "open browser" |
| Volume Up | "громче", "погромче", "volume up" |
| Volume Down | "тише", "потише", "volume down" |
| Set Volume | "громкость 50", "volume 50" |
| Mute | "без звука", "mute" |
| Screenshot | "скриншот", "screenshot" |
| Lock Screen | "заблокируй", "lock" |
| Sleep Mode | "спящий режим", "sleep" |
| Pause Media | "пауза", "pause", "стоп" |
| Play Media | "воспроизведи", "play" |
| Next Track | "следующий", "next" |
| Previous Track | "предыдущий", "previous" |
| Mouse Move | "мышь влево", "mouse left" |
| Mouse Click | "клик", "click" |
| System Status | "статус пк", "system status" |
| List Windows | "сколько окон", "list windows" |
| Open Notepad | "открой блокнот", "open notepad" |
| Open Calculator | "открой калькулятор", "open calc" |
| Open Explorer | "открой проводник", "open explorer" |
| Open Settings | "открой настройки", "open settings" |
| Open Task Manager | "открой диспетчер", "task manager" |

---

## Connection Lifecycle

```
1. Connect to ws://<ip>:8765
2. Send auth message
3. Receive auth_response
4. If success:
   a. Send commands/chat as needed
   b. Receive responses and events
   c. Send ping every 30s for keepalive
5. On disconnect: cleanup automatic
```

### Reconnection Strategy

```kotlin
// Recommended: Exponential backoff
var delay = 1000 // ms
val maxDelay = 30000

while (!connected) {
    try {
        connect()
        delay = 1000 // reset on success
    } catch (e: Exception) {
        delay = min(delay * 2, maxDelay)
        Thread.sleep(delay)
    }
}
```

---

## Example: Complete Flow

### Mobile sends voice command

```json
// 1. Auth
→ {"type":"auth","token":"ABCD-EFGH-IJKL-MNOP","device_id":"phone1","device_name":"Pixel 6"}
← {"type":"auth_response","success":true,"message":"Authentication successful","session_id":"session_123"}

// 2. Send chat (voice transcription)
→ {"type":"chat","id":"m1","text":"открой ютуб"}

// 3. Receive response (command was executed + AI response)
← {"type":"chat_response","id":"m1","text":"Открываю YouTube\n\n✅ Открываю YouTube","is_complete":true}

// 4. Keepalive
→ {"type":"ping","timestamp":1705123456789}
← {"type":"pong","timestamp":1705123456789}
```

---

## Rate Limits

| Type | Limit |
|------|-------|
| Auth attempts | 5 per minute per IP |
| Messages | 100 per minute per session |
| Commands | 30 per minute per session |
| Audio data | 1 MB per message |

Exceeding limits returns:
```json
{
  "type": "error",
  "code": "RATE_LIMITED",
  "message": "Too many requests"
}
```

---

## Security Notes

1. **Token Security** — Treat auth token like a password
2. **Local Only** — Never expose port 8765 to internet
3. **Same Network** — Devices must be on same LAN/WiFi
4. **No Encryption** — WebSocket is unencrypted (ws://, not wss://)
   - Acceptable for LAN; add TLS if extending to WAN

---

## SDK / Client Libraries

### Kotlin (Android)

```kotlin
val client = OkHttpClient()
val request = Request.Builder()
    .url("ws://192.168.1.100:8765")
    .build()

val webSocket = client.newWebSocket(request, object : WebSocketListener() {
    override fun onOpen(ws: WebSocket, response: Response) {
        val auth = """{"type":"auth","token":"XXXX","device_id":"phone1","device_name":"My Phone"}"""
        ws.send(auth)
    }
    
    override fun onMessage(ws: WebSocket, text: String) {
        val msg = Json.parseToJsonElement(text)
        // Handle message...
    }
})
```

### TypeScript (Node.js)

```typescript
import WebSocket from 'ws';

const ws = new WebSocket('ws://192.168.1.100:8765');

ws.on('open', () => {
    ws.send(JSON.stringify({
        type: 'auth',
        token: 'XXXX-XXXX-XXXX-XXXX',
        device_id: 'node-client',
        device_name: 'Test Client'
    }));
});

ws.on('message', (data) => {
    const msg = JSON.parse(data.toString());
    console.log('Received:', msg);
});
```

### Python

```python
import asyncio
import websockets
import json

async def connect():
    uri = "ws://192.168.1.100:8765"
    async with websockets.connect(uri) as ws:
        # Auth
        await ws.send(json.dumps({
            "type": "auth",
            "token": "XXXX-XXXX-XXXX-XXXX",
            "device_id": "python-client",
            "device_name": "Test"
        }))
        
        response = await ws.recv()
        print(json.loads(response))
        
        # Send chat
        await ws.send(json.dumps({
            "type": "chat",
            "id": "msg1",
            "text": "Hello ARIS"
        }))
        
        response = await ws.recv()
        print(json.loads(response))

asyncio.run(connect())
```
