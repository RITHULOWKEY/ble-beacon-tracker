# BLE Beacon Based Indoor Location Tracking System (Simulation)

Cinematic 3D digital-twin simulation built with HTML, CSS, JavaScript and Three.js.
All Bluetooth data is SIMULATED by a mathematical model. No Bluetooth hardware is used or read.

## Run it

1. Unzip the folder `ble-beacon-tracker`.
2. Double-click `index.html` (or drag it into Chrome, Edge or Firefox).
   No server, build step or internet connection is needed: Three.js is stored in `js/lib`.

Optional local server (only if your browser blocks something):
    python -m http.server 8000      then open http://localhost:8000

## Folder structure

    ble-beacon-tracker/
    |-- index.html          page structure, dashboard markup, explanation text
    |-- css/
    |   `-- style.css       layout and visual design
    `-- js/
        |-- lib/
        |   |-- three.min.js       Three.js r147 (classic build)
        |   `-- OrbitControls.js   mouse camera controls for Three.js r147
        |-- config.js       building, beacons, test paths, furniture, default parameters
        |-- ble-model.js    path-loss RSSI model, noise, filter, positioning algorithms
        |-- scene.js        everything drawn with Three.js: building, furniture, avatar, beacons,
        |                   effects and the camera director (shots, tour, transitions)
        |-- dashboard.js    sidebar UI, floor plan, RSSI chart
        `-- main.js         simulation loop, movement, BLE scans, control handlers

## Controls

- Start / Pause / Reset: run, freeze or restart the simulation.
- Test path: Full patrol, Corridor sweep, North wing, South wing, or the Beacon handoff test. Changing the path restarts the walk on it.
- Automatic walk: the receiver follows the selected path. Manual movement: W A S D, arrow keys or the on-screen pad (walls and solid furniture block you).
- Camera dock: Overview, Plan, Orbit, Indoor, Corridor, Beacons (close-ups of B1, B2, B3), Follow (tracks the avatar).
  "Cinematic tour" plays all of them in sequence. "Reset camera" returns the current shot to its home pose.
  Drag = orbit, scroll = zoom, right-drag = pan. Dragging during an automatic shot gives you control (Free camera).
- Display toggles: circles, trail, rings, lines, coverage glow, 3D labels, noise cloud, handoff effects, low walls.
- Render quality: Auto adapts resolution and shadows to your frame rate; High, Balanced and Low are fixed.
- Sliders: signal noise, beacon transmission power, path-loss exponent, smoothing, walking speed.
- Positioning algorithm: Gauss-Newton refinement (default), linear trilateration, weighted centroid.

## What the 3D scene shows (all simulated)

- Yellow avatar and ring: TRUE position. Pink hologram: ESTIMATED position. White line: the position error.
- Pink dots near the estimate: the last raw (unfiltered) per-scan estimates. Their spread is the measurement noise.
- Beacon labels show the simulated RSSI, the estimated distance and the noise realised in that scan.
- A pulse plays and a notice appears when the strongest beacon changes (stable for three scans).
- RSSI ignores walls in this model, so coverage glow and rings pass through them.

## Troubleshooting: page shows as unstyled plain HTML

That means the browser could not find `css/style.css` and the scripts. The usual cause is a changed folder layout
(for example `index.html` copied out of its folder, opened from inside a zip preview, or the files extracted flat).

1. Extract the whole zip and keep this structure: `index.html`, `css/style.css`, `js/*.js`, `js/lib/*.js`.
   A flat copy (every file next to `index.html`) also works, but `three.min.js` and `OrbitControls.js` must be there too
   (or you must be online, then Three.js is loaded from a CDN).
2. If something is still missing, a red message at the top of the page names the files.
3. Run a local server inside the folder: `python -m http.server 8000`, then open http://localhost:8000
4. If the 3D view says it could not start, enable hardware acceleration / WebGL in your browser.
   The dashboard and simulation keep working without it. Press F12 and open the Console for details.
