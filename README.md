Kaewai Vision – AI Object Detection Assistant
Real‑time camera‑based object detection with voice feedback for accessibility and safety.

📖 Project Overview:

Kaewai Vision is a fully browser‑based, no‑backend web application that uses your device’s camera to detect objects in real time and announce them out loud using the Web Speech API. It’s built around two core user scenarios:

 General awareness – anyone who wants an intelligent “second pair of eyes” that can identify objects around them.

 Assistive technology – particularly a Blind Assist Mode that provides detailed spatial guidance (e.g., “person on the left, very close”) to help visually impaired users navigate their surroundings.

The application loads a pre‑trained COCO‑SSD machine learning model directly in the browser (via TensorFlow.js), performs inference on every camera frame, draws bounding boxes, and alerts the user both visually and audibly. The entire stack lives in a single HTML file with zero installs, no server, and no sign‑up – just open it in a modern browser and grant camera access.

🎯 Key Objectives:

Real‑time object detection using a lightweight yet accurate deep‑learning model.

Voice feedback with configurable cooldowns to avoid repetition.

Two operating modes – a standard mode and an accessibility‑focused “Blind Assist” mode.

Beautiful, responsive UI with a soft, professional design (rounded cards, Poppins font, blue aesthetic).

Dark mode and fully persistent settings.

Complete client‑side execution – privacy‑friendly, works offline after initial model load (model cached by browser).

🧱 Tech Stack:

Layer	Technology
Frontend	HTML5, CSS3, vanilla JavaScript (ES6+)
AI Model	COCO‑SSD (MobileNet v2 backbone) via TensorFlow.js
Camera	navigator.mediaDevices.getUserMedia() (Browser Camera API)
Voice	Web Speech API (SpeechSynthesis)
Dataset	Trained on Microsoft COCO (Common Objects in Context) 2017
Hosting	Any static server, GitHub Pages, or local index.html
Everything runs in the browser; no Python, Node, or cloud API required.

🔬 How the Object Detection Works:

Model loading – When the app starts, TensorFlow.js fetches the COCO‑SSD model (about 10‑15 MB). Once loaded, it is stored in memory and ready for inference.

Camera feed – The browser captures a live video stream from the user’s webcam/camera.

Detection loop – Using requestAnimationFrame, the app continuously grabs the current video frame and passes it to model.detect(video).

Filtering – Predictions are filtered by a configurable confidence threshold (default 55%) to avoid noisy results.

Bounding boxes – Scaled coordinates are drawn on a <canvas> overlay directly on top of the video, with colour‑coded boxes (red for persons, orange for vehicles, green for animals).

Real‑time updates – The right panel lists all detected objects with their confidence scores and animated progress bars.

Important – The model runs entirely on the user’s CPU/GPU via WebGL; no image leaves the device.

🔊 How Voice Alerts Work:

The SpeechSynthesis API converts any text string into spoken audio using the system’s available voices.

Kaewai Vision builds intelligent phrases based on the current mode:

Normal Mode: “Person and car detected.”

Blind Assist Mode: “Person on the left, very close. Car straight ahead, nearby.”

To prevent alert fatigue, a cooldown system remembers the last time each object class was announced. The same object won’t be spoken again until the cooldown (configurable, default 5 seconds) passes. Blind Assist Mode shortens this cooldown for critical objects.

Distance and position are estimated purely from the bounding box:

Distance: ratio of the box area to the total frame area (>35% = very close, <5% = far away).

Position: horizontal centre of the box relative to the frame width (<30% = left, >70% = right).

🎛️ Modes Explained:

Mode	Description
Normal Mode	Standard object detection. Voice announcements are less frequent; visual alerts focus on people and vehicles.
Blind Assist	Designed for visually impaired users. Voice cooldown is reduced. Phrases include position and distance estimates. Alert banner is more assertive.
Switching modes resets all cooldown timestamps to give immediate new feedback.

📱 User Interface Design:

Navbar: Sticky glass‑morphism header with logo, navigation (Home, Detect, Settings), and dark‑mode toggle.

Left Panel: Voice toggle, mode selector buttons.

Center: Live camera feed with overlaid detection boxes and a contextual alert banner.

Right Panel: Live list of detected objects, each with an emoji, confidence percentage, and a coloured confidence bar.

Settings Panel: Collapsible drawer to tweak:

Confidence threshold (30%–90%)

Detection interval (150ms–800ms)

Voice speed (0.5x–1.8x)

Voice cooldown (2s–15s)

Footer: Camera status (green dot), AI model status, and a dataset badge.

Responsive: Fully adapts to mobile, tablet, and desktop.

🌐 How to Run Locally:

Download the single index.html file (the full code provided above).

Open it in Google Chrome, Edge, or any Chromium‑based browser (Firefox works too, but voice variety may be limited).

Allow camera access when the browser prompts.

Wait a few seconds for the COCO‑SSD model to download (you’ll see a loading message). After that, detection starts automatically.

Optional – Upload to GitHub Pages / Netlify / Vercel for a live demo.

🔧 Key Code Structure:

text
index.html
├── <style>         → all CSS with light/dark theme variables
├── <nav>           → sticky navigation bar
├── .settings-panel → collapsible controls (threshold, intervals, voice config)
├── main-container
│   ├── .left-panel  → controls (voice toggle, mode buttons)
│   ├── .center-area → video + canvas + alert banner
│   └── .right-panel → detected objects list
├── <footer>        → status indicators
└── <script>
    ├── DOM references
    ├── State variables (model, stream, cooldowns)
    ├── Camera initialization (getUserMedia)
    ├── Model loading (cocoSsd.load)
    ├── Detection loop (requestAnimationFrame + throttling)
    ├── Drawing (bounding boxes, labels)
    ├── Voice alerts (SpeechSynthesis + cooldown logic)
    ├── Visual alerts (alert banner logic)
    ├── Settings synchronization
    └── Init & event listeners
    
🧪 Accuracy & Real Dataset:

The underlying model is COCO‑SSD (Single Shot MultiBox Detector) trained on the COCO 2017 dataset – one of the most widely used object detection benchmarks, containing over 120k images across 80 everyday object classes. This means the model can reliably detect:

People, cars, trucks, bicycles, dogs, cats, birds

Furniture (chairs, tables, sofas)

Electronics (laptops, TVs, cell phones)

Kitchen items (bottles, cups, forks)

And many more

The model architecture (mobilenet_v2) is chosen to balance speed and accuracy, suitable for real‑time browser inference.

🔮 Future Enhancements:\

Custom object recognition (transfer learning with Teachable Machine)

Multi‑language voice output

Sound notification tones for different object categories

Recording & playback of detection sessions

PWA support for installing on mobile home screen

Using the back camera on mobile (already set via facingMode: 'environment')

💎 Why This Project Stands Out:

Resume‑ready: Demonstrates real‑world AI integration, web APIs, responsive design, and accessibility awareness.

100% client‑side: Privacy‑preserving, no data leaves the device.

Polished UI/UX: Not just a proof‑of‑concept – it’s designed with care, animations, and a consistent design language.

Assistive technology focus: Shows empathy and problem‑solving beyond purely technical execution.

