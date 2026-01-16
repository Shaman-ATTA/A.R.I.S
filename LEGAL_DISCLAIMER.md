# Legal Disclaimer

## A.R.I.S. — Advanced Research & Intelligence System

**Last Updated:** January 2026

---

## Disclaimer of Warranties

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.

IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

## Limitation of Liability

To the maximum extent permitted by applicable law, in no event shall the author or contributors be liable for any special, incidental, indirect, or consequential damages whatsoever (including, but not limited to, damages for loss of profits, business interruption, loss of information, or any other pecuniary loss) arising out of the use of or inability to use the Software.

---

## Third-Party Services

### Google Gemini API

This software integrates with Google Gemini API. By using A.R.I.S., you agree to:
- [Google Terms of Service](https://policies.google.com/terms)
- [Google Generative AI Terms](https://ai.google.dev/terms)

The author is not responsible for:
- Google API availability or performance
- Changes to Google's pricing or terms
- Data processed by Google's AI services

### Microsoft Edge TTS

Text-to-speech functionality may use Microsoft Edge TTS services. Usage is subject to Microsoft's terms of service.

### Web Speech API

Voice recognition uses browser-provided Web Speech API. Audio may be processed by cloud services (Google, Microsoft) depending on the browser used.

---

## System Access

A.R.I.S. requires extensive system access to function, including:

- **Microphone access** — for voice commands
- **Screen capture** — for screen analysis feature
- **Keyboard/mouse control** — for automation commands
- **File system access** — for file operations
- **Network access** — for API communications
- **Process management** — for system control features

By using this software, you acknowledge and accept these access requirements.

---

## Security Considerations

### Local Network Only

The WebSocket server binds to all network interfaces (0.0.0.0). While designed for local network use only:

- Do NOT expose port 8765 to the internet
- Use firewall rules to restrict access
- The author is not responsible for unauthorized access due to network misconfiguration

### Token Security

The authentication token should be treated as a password:
- Do not share publicly
- Do not commit to version control
- Regenerate if compromised

### Command Execution

A.R.I.S. can execute system commands. The author is not responsible for:
- Unintended command execution
- System damage from commands
- Data loss from file operations

---

## Data Privacy

### Local Processing

A.R.I.S. processes most data locally. However:

- **Voice recognition** may send audio to Google/Microsoft cloud services
- **AI chat** sends text to Google Gemini API
- **Knowledge base** is stored locally on your device

### Data Retention

- Conversation history: Stored locally
- Knowledge base: Stored locally in SQLite
- Logs: Stored locally, rotated automatically
- No data is sent to the author's servers

---

## Intellectual Property

### Original Work

A.R.I.S. is an original work created by the author. The design, code, and functionality are independently developed.

### No Affiliation

A.R.I.S. is NOT affiliated with, endorsed by, or connected to:
- Marvel Entertainment
- Disney
- Any fictional AI assistants from movies or media
- Google (beyond API usage)
- Microsoft (beyond API usage)

### Third-Party Code

This software includes third-party open-source libraries. See [DEPENDENCIES.md](DEPENDENCIES.md) for complete list and licenses.

---

## Compliance

### User Responsibility

Users are responsible for ensuring their use of A.R.I.S. complies with:
- Local laws and regulations
- Workplace policies
- Terms of service of integrated services
- Privacy regulations (GDPR, CCPA, etc.)

### Prohibited Uses

Do not use A.R.I.S. for:
- Unauthorized access to systems
- Circumventing security measures
- Harassment or surveillance
- Any illegal activities

---

## Modifications

The author reserves the right to modify this disclaimer at any time. Continued use of the software constitutes acceptance of any modifications.

---

## Governing Law

This disclaimer shall be governed by and construed in accordance with the laws of the Russian Federation, without regard to its conflict of law provisions.

---

## Contact

For legal inquiries:

**Email:** yrekkomar@gmail.com

---

**By using A.R.I.S., you acknowledge that you have read, understood, and agree to be bound by this disclaimer.**
