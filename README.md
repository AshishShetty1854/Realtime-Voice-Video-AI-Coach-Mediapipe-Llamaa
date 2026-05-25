# 🏋️ Realtime Voice & Video AI Gym Coach

An AI-powered **real-time fitness coaching system** built with Python, Streamlit, MediaPipe, and Groq. It watches you exercise through your webcam, tracks your reps and form automatically using pose estimation, and delivers live **voice feedback** from an LLM-based AI coach — all in your browser.

---

## 📌 Overview

This project combines computer vision and large language models to create a truly interactive gym companion. It goes beyond simple rep counting — the AI coach actively monitors your posture, detects form errors, and speaks to you during your workout using text-to-speech powered by the Groq API.

---

## ✨ Features

- 🎥 **Real-Time Webcam Streaming** — Live video via `streamlit-webrtc` with async processing
- 🦴 **Pose Estimation** — MediaPipe Pose tracks 33 body landmarks per frame
- 🔢 **Automatic Rep Counting** — Joint angle analysis triggers rep increments accurately
- 📐 **Per-Exercise Form Metrics** — Tracks exercise-specific angles, alignment, and posture cues
- 🗣️ **LLM Voice Coaching** — Groq-powered coach speaks feedback at key moments (start, set complete, workout done)
- 🔊 **Text-to-Speech** — gTTS converts coach responses to autoplay audio in the browser
- 👤 **User Authentication** — Login wall with per-user session management
- 📊 **Workout History** — SQLite persistence; view aggregated reps, sets, and time per session
- 🎯 **Workout Planning** — Choose exercise, sets, and reps per set before starting
- 💅 **Custom Styling** — CSS theming with a local Adobe Clean font

---

## 🏃 Supported Exercises

| Exercise | Tracked Metrics |
|---|---|
| **Squats** | Knee angle, back angle, depth status |
| **Push-ups** | Elbow angle, body alignment, hip position |
| **Biceps Curls (Dumbbell)** | Elbow angle, shoulder stability, swing detection |
| **Shoulder Press** | Elbow angle, arm extension, back arch |
| **Lunges** | Front knee angle, torso angle, balance status |

---

## 🗂️ Project Structure

```
Realtime-Voice-Video-AI-Coach-Mediapipe-Llamaa/
│
├── main.py                        # Streamlit app entry point
├── requirements.txt               # Python dependencies
├── packages.txt                   # System-level packages (for deployment)
│
├── core/                          # Core app logic
├── detectors/                     # Exercise-specific pose detectors
├── ml_models/                     # ML model utilities
│
├── services/
│   ├── auth/
│   │   └── login_wall.py          # User authentication
│   ├── coaching/
│   │   ├── llm.py                 # LLMCoach — Groq-based coaching logic
│   │   ├── tts.py                 # TextToSpeech — gTTS wrapper
│   │   └── voice_pipeline.py      # VoicePipeline — event-driven coaching triggers
│   ├── config/
│   │   └── workout_config.py      # Exercise options and constants
│   ├── persistence/
│   │   └── exercise_repository.py # SQLite DB init and workout history queries
│   ├── state/
│   │   └── session_defaults.py    # Streamlit session state initialization
│   ├── tracking/
│   │   └── metrics.py             # Syncs WebRTC video processor metrics to session state
│   ├── ui/
│   │   └── style_loader.py        # CSS + font injection helpers
│   └── vision/
│       └── exercise_video_processor.py  # WebRTC VideoProcessor — pose + rep logic
│
├── static/
│   ├── style.css                  # App-wide custom CSS
│   └── AdobeClean.otf             # Local font
│
└── .streamlit/                    # Streamlit configuration
```

---

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Language | Python 3.10+ |
| Web Framework | Streamlit 1.54 |
| Live Video | streamlit-webrtc 0.64.5 |
| Pose Estimation | MediaPipe 0.10.14 |
| Computer Vision | OpenCV (headless) 4.10 |
| LLM / AI Coaching | Groq API (`groq >= 0.12.0`) |
| Text-to-Speech | gTTS 2.5.3 |
| Data | Pandas 2.2.3, SQLite |
| Config | python-dotenv 1.2.2 |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- A working webcam
- A [Groq API key](https://console.groq.com/) (free tier available)

### 1. Clone the Repository

```bash
git clone https://github.com/AshishShetty1854/Realtime-Voice-Video-AI-Coach-Mediapipe-Llamaa.git
cd Realtime-Voice-Video-AI-Coach-Mediapipe-Llamaa
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

> On Linux/Ubuntu, you may also need the system packages listed in `packages.txt`:
> ```bash
> sudo apt-get install $(cat packages.txt | tr '\n' ' ')
> ```

### 4. Set Up Environment Variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Alternatively, if deploying to Streamlit Cloud, add it under **Settings → Secrets**:

```toml
GROQ_API_KEY = "your_groq_api_key_here"
```

### 5. Run the App

```bash
streamlit run main.py
```

Open your browser at `http://localhost:8501`.

---

## 🎮 How to Use

1. **Log in** using the authentication screen.
2. In the **sidebar**, select your exercise, number of sets, and reps per set.
3. Click **Start Workout** — the webcam stream activates and the AI coach greets you.
4. Perform your exercise in front of the camera. Keep your full body visible.
5. Watch the **sidebar metrics** update in real time (angles, alignment, depth, etc.).
6. The coach will speak feedback automatically at key moments — workout start, set completion, and workout end.
7. Click **End Workout** when done. Your session is saved to history automatically.
8. Scroll down to view your **Workout History** table with aggregated stats.

---

## ☁️ Deploying to Streamlit Cloud

1. Push the repo to GitHub.
2. Go to [streamlit.io/cloud](https://streamlit.io/cloud) and connect your repo.
3. Set `main.py` as the entry point.
4. Add `GROQ_API_KEY` under **App Settings → Secrets**.
5. Deploy — Streamlit Cloud will install `requirements.txt` and `packages.txt` automatically.

---

## 🧠 Architecture: Voice Coaching Pipeline

The voice pipeline is event-driven. Key events are:

| Event | Trigger |
|---|---|
| `workout_started` | User clicks "Start Workout" |
| `set_completed` | Rep count reaches `reps_per_set` |
| `workout_completed` | User clicks "End Workout" |

On each event, `LLMCoach` sends a prompt to Groq (Llama 3), receives a coaching message, and `TextToSpeech` converts it to audio that autoplays in the browser.

---

## ⚠️ Disclaimer

This application is intended for general fitness guidance only. It is **not a substitute** for professional medical or fitness advice. Always consult a certified trainer or healthcare professional before starting a new exercise program, especially if you have pre-existing conditions or injuries.

---

## 🤝 Contributing

Contributions are welcome! To get started:

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 👤 Author

**Ashish Shetty**
- GitHub: [@AshishShetty1854](https://github.com/AshishShetty1854)

---

## 📄 License

This project is open-source. Please check the repository for license details.
