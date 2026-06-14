# 🎙️ Jarvis - Desktop Clap Listener & Workspace Automator

Jarvis is a lightweight, background-running desktop automation utility designed to streamline your daily engineering workspace setup. Using your default microphone, it listens for audio transients (claps) and dynamically launches your developmental environment. 

With deep Windows integration, Jarvis can focus/open editors in fullscreen mode, snap custom Chrome profiles to designated monitor screens, and greet you via high-quality text-to-speech engine.

---

## ✨ Core Features

*   **🎙️ Smart Transient Detection:** Listens to the default audio input device and dynamically tracks noise floor levels to trigger actions on a single clap or a double clap within a precise time window.
*   **🔊 Audio/Music Trigger:** Automatically opens custom Spotify or YouTube tracks (`SONG_URI`) to kick off your focus session.
*   **🌐 Chrome Profile Snap (Multi-Monitor):** Launches custom Google Chrome user profiles, loads designated URLs (e.g., Outlook, YouTube), and snaps/maximized them to specific monitors using Windows APIs.
*   **💬 High-Quality Vocal Greetings:** Greets you by name using Microsoft Edge's neural voice TTS engine (`edge-tts`). Audio files are cached locally to save API calls, with a native Windows SAPI fallback.
*   **💻 Dual Editor Control (Cursor & VS Code):** Spawns or brings existing instances of Cursor and VS Code to the foreground, sending keystrokes (like `F11`) automatically to enter immersive fullscreen modes.
*   **🏢 Teams Integration:** Automatically launches Microsoft Teams via native protocol handlers or executable shortcuts.
*   **🔕 Silent Background Run:** Includes VBScript wrappers to let Jarvis run invisibly in the background on startup.

---

## 📂 Project Structure

```text
├── jarvis.py              # Main python script containing logic and event loops
├── .env                   # Configuration file storing credentials and settings
├── requirements.txt       # Python package dependencies
├── run_jarvis.vbs         # Silent VBS launcher for the listener mode
└── trigger_jarvis.vbs     # Silent VBS launcher for immediate manual triggering
```

---

## 🛠️ Installation & Setup

Follow these steps to set up Jarvis on your Windows system:

### 1. Clone or Copy the Repository
Place all the project files in a folder on your local drive (e.g., `C:\Users\gurui\Downloads\jarvis-main (1)\jarvis-main`).

### 2. Set Up a Virtual Environment
It is highly recommended to isolate your Python workspace. Open command prompt or PowerShell and navigate to the project directory:

```bash
# Create a virtual environment named "myenv"
python -m venv myenv

# Activate the virtual environment
# In command prompt:
myenv\Scripts\activate
# In PowerShell:
.\myenv\Scripts\Activate.ps1
```

### 3. Install Dependencies
Install all required external libraries listed in `requirements.txt`:

```bash
pip install -r requirements.txt
```

---

## ⚙️ Configuration (`.env`)

Configure your workspace environment variables by editing the `.env` file in the root folder. Below are the settings you can customize:

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **`SINGLE_CLAP_ONLY`** | `Boolean` | `True` | Set to `True` for single-clap trigger, or `False` for double-clap. |
| **`EDGE_TTS_VOICE`** | `String` | `en-US-AvaNeural` | Neural voice identifier used for the voice greeting. |
| **`OPEN_CHROME_FULLSCREEN`** | `Boolean` | `True` | Instantly snap triggered Chrome profiles into fullscreen. |
| **`FOCUS_EXISTING_VSCODE_ON_DOUBLE_CLAP`** | `Boolean` | `True` | Foreground an existing VS Code window instead of spawning a new one. |
| **`OPEN_NEW_VSCODE_ON_DOUBLE_CLAP`** | `Boolean` | `False` | Force-open an additional new window for VS Code. |
| **`VSCODE_OPEN_FULLSCREEN`** | `Boolean` | `True` | Send an automatic `F11` keypress to VS Code to maximize/fullscreen it. |
| **`FOCUS_EXISTING_CURSOR_ON_DOUBLE_CLAP`** | `Boolean` | `False` | Focus/foreground an existing Cursor editor window. |
| **`OPEN_NEW_CURSOR_ON_DOUBLE_CLAP`** | `Boolean` | `False` | Force-open an additional new window for Cursor. |
| **`CURSOR_OPEN_FULLSCREEN`** | `Boolean` | `True` | Send an automatic `F11` keypress to Cursor to maximize/fullscreen it. |
| **`SONG_URI`** | `String` | *(empty)* | Optional Spotify URI or YouTube URL to play upon trigger. |
| **`JARVIS_WELCOME_ENABLED`** | `Boolean` | `True` | Enable voice feedback/welcome speech upon triggering. |
| **`JARVIS_WELCOME_PHRASE`** | `String` | `hey Welcome back Guru shankar. Here your workspace are opening now` | Customized spoken text. |
| **`JARVIS_WELCOME_CACHE_ENABLED`** | `Boolean` | `True` | Cache TTS audio to avoid calling API servers multiple times. |
| **`SPIKE_RATIO`** | `Float` | `3.5` | Threshold sensitivity (e.g. times louder than baseline) to register a clap. |

### Chrome Profiles Customization
You can customize Chrome profiles directly inside the `jarvis.py` script. Locate the `CHROME_PROFILES` array to configure which profiles are opened, what URL they point to, and which monitor index they snap to:

```python
CHROME_PROFILES = [
    {
        "name": "Default",
        "email": "guru.ikastar@gmail.com",
        "url": "https://outlook.cloud.microsoft/mail/",
        "description": "Outlook Mail",
        "monitor": 1,
    },
    # Add other profiles here
]
```

---

## 🚀 Usage

### Option A: Interactive Listener Mode (For testing sensitivity)
Run the script directly in your terminal to see real-time volume logging and threshold metrics:

```bash
python jarvis.py
```
*   *Tip:* Adjust `SPIKE_RATIO` and `MIN_RMS` in your configuration if it triggers on background noise or misses your claps.

### Option B: Manual Execution Mode (Trigger actions immediately)
If you want to immediately execute the browser, editor, and greeting actions without waiting for a clap, run:

```bash
python jarvis.py --trigger
```

---

## 🥷 Run Silently in the Background (Windows)

To run the application silently in the background (no visible terminal window), use the included VBScript files:

1.  **Double-click `run_jarvis.vbs`** to start the listener in background mode.
2.  **Double-click `trigger_jarvis.vbs`** to manually trigger the automation workflow immediately and silently.

### Autorun on Windows Startup
To have Jarvis start listening as soon as you log in:
1. Press `Win + R`, type `shell:startup`, and press Enter.
2. Create a shortcut of [run_jarvis.vbs](file:///c:/Users/gurui/Downloads/jarvis-main%20%281%29/jarvis-main/run_jarvis.vbs) and paste it into the Startup folder that opened.

---

## 🛠️ Troubleshooting

*   **PortAudioError:** Ensure your default recording device is enabled in the Windows sound settings, and your microphone drivers are up to date. You can change `SAMPLE_RATE` in `.env` to match your hardware rate (e.g., `48000`).
*   **VS Code / Cursor F11 Fullscreen Issues:** The script relies on Windows APIs to focus the editor and send keyboard events. If the editor fails to toggle fullscreen, ensure you don't have other elevated admin processes blocking lower-level input signals.
*   **Chrome Profile Snapping Multi-monitor Error:** Ensure Chrome is closed before running, or check that you configured the exact name folder corresponding to your profile folders (`Default`, `Profile 1`, etc.) located in `%LOCALAPPDATA%\Google\Chrome\User Data`.
