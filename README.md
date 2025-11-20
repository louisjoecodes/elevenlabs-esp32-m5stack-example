<img width="1920" height="1080" alt="Twitter-Post-Template" src="https://github.com/user-attachments/assets/ec1bbc79-0104-4d4d-868e-831dc0cb5fc5" />

# ElevenLabs ESP32 M5Stack Example

Build real-time voice AI agents that run directly on ESP32 microcontrollers with WebRTC support.

## Quick Start

### 1. Clone the Repository

**Important:** Use `--recursive` flag to fetch all required submodules:

```bash
git clone --recursive https://github.com/louisjoecodes/elevenlabs-esp32-m5stack-example.git
cd elevenlabs-esp32-m5stack-example
```

**Already cloned without `--recursive`?** Initialize submodules now:

```bash
git submodule update --init --recursive
```

### 2. Open in VS Code with PlatformIO

1.  Install the **PlatformIO IDE** extension for VS Code.
2.  **Open the specific folder for your device:**
    *   **AtomS3R:** `pipecat-esp32/esp32-m5stack-atoms3r`
    *   **CoreS3:** `pipecat-esp32/esp32-m5stack-cores3`
    *   **S3 Box 3:** `pipecat-esp32/esp32-s3-box-3`

### 3. Configure WiFi & Server Settings

Since we are using PlatformIO for a hackathon, the most reliable way to configure credentials is to hardcode them directly in the source file (this avoids complex shell escaping issues).

1.  Open `src/wifi.cpp` in your device folder.
2.  Edit the `HACKATHON_WIFI_SSID` and `HACKATHON_WIFI_PASS` defines at the top:

    ```cpp
    #define HACKATHON_WIFI_SSID "YourWiFiName"
    #define HACKATHON_WIFI_PASS "YourWiFiPassword"
    ```

    *   **For iPhone Hotspots:** Ensure "Maximize Compatibility" is ON in your phone settings (ESP32 requires 2.4GHz WiFi).

3.  Open `src/http.cpp` in your device folder.
4.  Edit the `HACKATHON_SERVER_URL` define at the top:

    ```cpp
    #define HACKATHON_SERVER_URL "http://YOUR_LAPTOP_IP:7860/api/offer"
    ```

    *   **Finding your IP:**
        *   **macOS:** `ipconfig getifaddr en0`
        *   **Linux:** `hostname -I`
        *   **Windows:** `ipconfig`

### 4. Build & Upload

1.  Click the **PlatformIO Alien icon** in the left sidebar.
2.  Expand your project tasks.
3.  Click **Upload and Monitor**.

### 5. Run the Python Server

Open a new terminal in the root directory:

```bash
cd server
cp .env.example .env
```

Edit `.env` and add your API keys:
```env
ELEVENLABS_API_KEY=your_key_here
ELEVENLABS_VOICE_ID=your_voice_id_here
OPENAI_API_KEY=your_key_here
```

Install dependencies and start the server:

```bash
# Using uv (fast)
uv sync
uv run bot.py --host 0.0.0.0 --esp32

# Or standard pip
pip install -r requirements.txt
python bot.py --host 0.0.0.0 --esp32
```

The server will start on port **7860**.

## 🔧 Troubleshooting

### Error: "Failed to resolve component 'srtp'"
**Solution:** Run `git submodule update --init --recursive` in the project root.

### Device Not Connecting to WiFi?
1.  **Check Frequency:** ESP32 only supports 2.4GHz. If using a phone hotspot, enable "Maximize Compatibility".
2.  **Check Password:** Verify the password in `src/wifi.cpp` is correct.
3.  **Check Logs:** Look at the Serial Monitor output in VS Code.

### Connection Timed Out (WebRTC)?
1.  **Check IP:** Ensure `src/http.cpp` has your computer's *current* local IP.
2.  **Check Firewall:** Your computer firewall might block the connection. Temporarily disable it or allow Python to accept incoming connections.
3.  **Test Server:** Run `curl http://YOUR_IP:7860/api/offer` from your terminal. If it hangs or fails, the server isn't reachable.

## Key Components
- **libpeer**: WebRTC implementation for embedded systems
- **libsrtp**: Secure Real-time Transport Protocol
- **esp-libopus**: Opus audio codec for ESP32

## 🔗 Related Projects
- [ElevenLabs](https://elevenlabs.io/docs/overview)
- [Pipecat](https://github.com/pipecat-ai/pipecat)
