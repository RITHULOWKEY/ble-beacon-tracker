# BLE Beacon Based Indoor Location Tracking System (Simulation)

[![Live Demo](https://img.shields.io/badge/Live_Demo-ble--beacon--tracker.vercel.app-00dfa2?style=for-the-badge&logo=vercel&logoColor=black)](https://ble-beacon-tracker.vercel.app/)
[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RITHULOWKEY/ble-beacon-tracker)
[![Three.js](https://img.shields.io/badge/Three.js-r147-black?style=for-the-badge&logo=three.js&logoColor=white)](https://threejs.org/)
[![Status](https://img.shields.io/badge/Build-Passing-brightgreen?style=for-the-badge)](https://ble-beacon-tracker.vercel.app/)

A cinematic **3D digital-twin indoor positioning simulation** built with Three.js, Vanilla CSS, and modern JavaScript. It visualizes how Bluetooth Low Energy (BLE) beacons, log-distance path-loss models, signal noise, and multilateration algorithms work together to track assets and personnel indoors.

🔗 **Try the live simulation here**: [**https://ble-beacon-tracker.vercel.app/**](https://ble-beacon-tracker.vercel.app/)

---

> [!NOTE]
> **Pure Mathematical Simulation**: No physical Bluetooth hardware is required. All beacon broadcasts, RSSI readings, path loss, multipath fading, and Gaussian measurement noise are simulated mathematically in real time.

---

## 🌟 Key Features

### 🏢 1. 3D Architectural Digital Twin
- Interactive multi-room floor plan with realistic walls, doorways, reception furniture, desks, and ambient lighting.
- Visualized BLE beacons (**B1, B2, B3**) with active radio propagation waves and coverage glow.
- Real-time ground truth (**Yellow Avatar**) vs. calculated trilateration position (**Pink Hologram**).
- Dynamic error vector line showing instantaneous Euclidean positioning drift and error margin.

### 📐 2. Real-Time Positioning Algorithms
Compare three positioning methods on the fly:
- **Gauss-Newton Non-Linear Least Squares** *(Default)*: Iterative optimization solving the non-linear distance intersection equations with minimal residual error.
- **Linear Least-Squares Trilateration**: Matrix algebraic solution subtracting reference equations to linearize circle intersections.
- **Weighted Centroid Localization (WCL)**: Fast centroid calculation weighting beacon positions by inverse estimated distance or RSSI power.

### 📡 3. Realistic Radio Frequency (RF) Simulation
- **Log-Distance Path Loss Model**: Calculates received signal strength based on distance and environmental absorption.
- **Configurable Gaussian Noise ($\sigma$)**: Simulates indoor RF interference, multipath fading, and antenna variability.
- **Exponential Moving Average (EMA) Filter**: Smooths erratic raw RSSI measurements over successive scans.
- **Beacon Handoff Engine**: Detects transitions when the mobile unit moves into proximity of a new dominant beacon.

### 🎥 4. Cinematic Camera Director
- **Multiple Preset Views**:
  - `Overview`: Wide isometric architectural perspective.
  - `Plan`: Top-down orthographic-style 2D/3D floor plan.
  - `Orbit`: Smooth cinematic revolution around the facility.
  - `Indoor`: Cutaway perspective focused on room interiors.
  - `Corridor`: First-person/dolly track down the main hallway.
  - `Beacons`: Zoomed-in telemetry inspection of beacons B1, B2, and B3.
  - `Follow`: Dynamic tracking camera linked to the walking receiver.
- **Cinematic Tour Mode**: Plays all camera choreography automatically in sequence.
- **Free Camera**: Orbit, pan, and zoom freely with mouse or touch controls.

### 📊 5. Comprehensive Telemetry & Analytics Dashboard
- **Live 2D Mini-Map**: Top-down radar view with true and estimated coordinates, beacon positions, and trail histories.
- **Real-Time RSSI Oscilloscope**: Live multi-channel signal graph showing decibel variations for B1, B2, and B3.
- **Live Statistics**: Real-time Euclidean error (meters), scan rate (Hz), current room zone, and dominant beacon ID.
- **Interactive Control Sliders**:
  - Signal noise ($\sigma$)
  - Beacon TX Power ($A_0$ at 1 meter)
  - Path Loss Exponent ($n$)
  - Filter Smoothing Factor ($\alpha$)
  - Receiver Walking Speed (m/s)

---

## 🔬 Mathematical Principles

### 1. Log-Distance Path Loss Model
The simulated Received Signal Strength Indication (RSSI) is computed using:

$$\text{RSSI}(d) = A_0 - 10 \cdot n \cdot \log_{10}(d) + X_\sigma$$

Where:
- $d$: True Euclidean distance between receiver and beacon (meters)
- $A_0$: Reference RSSI at 1 meter distance (typically $-59 \text{ dBm}$)
- $n$: Path loss exponent ($2.0$ for free space, $2.5 - 3.5$ for office interiors)
- $X_\sigma \sim \mathcal{N}(0, \sigma^2)$: Zero-mean Gaussian random noise representing multipath fading and shadow effects.

### 2. Distance Estimation from RSSI
Inverting the path loss model yields the estimated distance:

$$\hat{d} = 10^{\frac{A_0 - \text{RSSI}}{10 \cdot n}}$$

### 3. Trilateration Formulation
Given beacon coordinates $(x_i, y_i)$ and estimated distances $\hat{d}_i$ for $i \in \{1, 2, 3\}$, the system solves:

$$(x - x_i)^2 + (y - y_i)^2 = \hat{d}_i^2$$

---

## 🕹️ Controls

| Control | Action |
| :--- | :--- |
| **Start / Pause / Reset** | Start, freeze, or restart the simulation loop |
| **Path Selection** | Choose between `Full patrol`, `Corridor sweep`, `North wing`, `South wing`, or `Handoff test` |
| **Manual Movement** | Use `W`, `A`, `S`, `D` or Arrow keys (collision detection prevents wall penetration) |
| **Camera Navigation** | Left Click + Drag to rotate, Right Click + Drag to pan, Scroll to zoom |
| **Display Toggles** | Toggle coverage rings, error lines, true/estimated trails, and beacon labels |

---

## 📁 Repository Structure

```text
ble-beacon-tracker/
│
├── index.html              # Main simulation dashboard & 3D canvas host
├── README.md               # Project documentation & reference
│
├── css/
│   └── style.css           # Glassmorphism dark-mode UI styling
│
└── js/
    ├── config.js           # Beacon coordinates, building blueprints, test paths
    ├── ble-model.js        # Path-loss math, Gaussian noise, trilateration algorithms
    ├── scene.js            # Three.js 3D world: lighting, materials, avatar, cameras
    ├── dashboard.js        # 2D radar minimap, live charts, telemetry readout
    ├── main.js             # Simulation tick loop, collision detection, input handlers
    │
    └── lib/
        ├── three.min.js    # Three.js 3D library (r147)
        └── OrbitControls.js# Three.js camera control module
```

---

## 🚀 Local Setup & Running Locally

This project requires **zero build tools, bundlers, or package installations**. It runs entirely in modern web browsers:

### Option 1: Direct Browser Launch
Simply double-click `index.html` or drag it into any modern web browser (Google Chrome, Microsoft Edge, Firefox, or Safari).

### Option 2: Local HTTP Server (Recommended)
To run with a lightweight local web server:

```bash
# Using Python 3:
python -m http.server 8000
```

Then visit:
```
http://localhost:8000
```

---

## 🌐 Deployment

The application is deployed on **Vercel** with continuous deployment linked to this repository:
- **Production URL**: [https://ble-beacon-tracker.vercel.app/](https://ble-beacon-tracker.vercel.app/)

To deploy your own copy:
1. Fork or clone this repository:
   ```bash
   git clone https://github.com/RITHULOWKEY/ble-beacon-tracker.git
   ```
2. Import the repository into [Vercel](https://vercel.com/new).
3. Vercel will automatically recognize the static assets and deploy in seconds!

---

## 🛠️ Built With

- **[Three.js (r147)](https://threejs.org/)** – 3D rendering, lighting, materials, and procedural scene generation.
- **HTML5 Canvas & SVG** – Real-time 2D floor plan radar and dynamic signal waveforms.
- **Vanilla JavaScript (ES6+)** – Multilateration solvers, matrix algebra, and event loops.
- **CSS3** – Glassmorphism HUD, responsive layout, and dark aesthetic design.
- **[Vercel](https://vercel.com/)** – Cloud hosting and global edge delivery.

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
