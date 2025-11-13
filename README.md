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

### 2. Install ESP-IDF

Install the ESP-IDF toolchain v5.4+ following the [official installation guide](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/linux-macos-setup.html).

**Important:** After installing ESP-IDF, verify submodules are initialized:

```bash
cd $IDF_PATH
git submodule update --init --recursive
```

### 3. Set Up Your Environment

Load ESP-IDF tools in your terminal:

```bash
source $IDF_PATH/export.sh
```

Find your local IP address (the ESP32 will use this to connect to your server):

**macOS:**
```bash
ipconfig getifaddr en0
```

**Linux:**
```bash
hostname -I | awk '{print $1}'
```

Configure your WiFi and Pipecat server URL with your IP address:

```bash
export WIFI_SSID="YourNetworkName"
export WIFI_PASSWORD="YourPassword"
export PIPECAT_SMALLWEBRTC_URL="http://YOUR_IP:7860/api/offer"
```

Replace `YOUR_IP` with the IP address from above (e.g., `http://192.168.1.10:7860/api/offer`).

### 4. Build for Your Device

Navigate to your device directory:

```bash
# For M5Stack AtomS3R
cd esp32-m5stack-atoms3r
```

Set the target and build:

```bash
idf.py --preview set-target esp32s3
idf.py build
```

### 5. Flash to Device

**macOS:**
```bash
idf.py flash
```

**Linux:**
```bash
idf.py -p /dev/ttyACM0 flash
```

If on linux: Find your device port with `ls /dev/tty*` or `sudo dmesg | grep tty`.

### 6. Monitor Serial Output

```bash
idf.py monitor
```

Press `Ctrl+]` to exit the monitor.

## ▶️ Usage

### Running a Pipecat Bot with ElevenLabs TTS, Deepgram STT + OpenAI

1. **Set up the server environment:**

   ```bash
   cd server
   cp .env.example .env
   ```

   Edit `.env` and add your API keys:
   ```
   DEEPGRAM_API_KEY=your_key_here
   ELEVENLABS_API_KEY=your_key_here
   ELEVENLABS_VOICE_ID=your_voice_id_here
   OPENAI_API_KEY=your_key_here
   ```

2. **Install dependencies and start the server:**

   ```bash
   uv sync
   uv run bot.py --host 0.0.0.0 --esp32
   ```

   The server will start on port 7860. Keep this terminal running.

3. **Connect your ESP32:**

   Your device should now connect to WiFi and establish a WebRTC connection to your Pipecat bot!

   If you haven't flashed your device yet, run:
   ```bash
   idf.py flash monitor
   ```

## 🔧 Troubleshooting

### Error: "Failed to resolve component 'srtp'"

**Cause:** Git submodules weren't initialized.

**Solution:**
```bash
cd /path/to/pipecat-esp32
git submodule update --init --recursive
```

### Error: "Include directory ... is not a directory"

**Cause:** ESP-IDF submodules are incomplete.

**Solution:**
```bash
cd $IDF_PATH
git submodule update --init --recursive
```

### Error: "Directory 'build' doesn't seem to be a CMake build directory"

**Cause:** Incomplete build artifacts.

**Solution:**
```bash
rm -rf build
idf.py --preview set-target esp32s3
```

### Device Not Connecting to WiFi

1. Double-check your WiFi credentials:
   ```bash
   echo $WIFI_SSID
   echo $WIFI_PASSWORD
   ```

2. Ensure your network is 2.4GHz (ESP32 doesn't support 5GHz)

3. Check serial output for connection errors:
   ```bash
   idf.py monitor
   ```

### WebRTC Connection Fails

1. Verify the Pipecat server is running and accessible

2. Test the URL from your development machine:
   ```bash
   curl http://YOUR_IP:7860/api/offer
   ```

3. Ensure ESP32 and server are on the same network

4. Check firewall rules aren't blocking port 7860

The M5Stack projects reference shared components from `esp32-s3-box-3/components/`.

## Key Components

- **libpeer**: WebRTC implementation for embedded systems
- **libsrtp**: Secure Real-time Transport Protocol
- **esp-libopus**: Opus audio codec for ESP32
- **esp_codec_dev**: Espressif audio codec driver


## 🔗 Related Projects

- [ElevenLabs](https://elevenlabs.io/docs/overview)
- [Pipecat](https://github.com/pipecat-ai/pipecat) - Framework for building voice and multimodal AI agents

---
