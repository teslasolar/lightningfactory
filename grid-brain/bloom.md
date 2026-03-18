# GRID BRAIN

**Bloom Prompt · GitHub Pages App · Lightning Factory Directory**

`teslasolar.github.io/lightning-factory/grid-brain/`

---

## META

| Key | Value |
|-----|-------|
| name | grid-brain |
| repo | teslasolar/lightning-factory |
| path | /grid-brain/ |
| type | GitHub Pages directory (static HTML/JS/CSS) |
| deploy | teslasolar.github.io/lightning-factory/grid-brain/ |
| purpose | interactive tools for understanding power grid EMF vs brainwaves |
| backend | none for Phase 1, API stub for future hardware integration |
| stack | vanilla HTML/JS, Web Audio API, Canvas, zero dependencies |

---

## DIRECTORY STRUCTURE

```
lightning-factory/
├── index.html                    # existing site root
├── grid-brain/
│   ├── index.html                # directory hub (links all tools)
│   ├── api.js                    # API stub (localStorage now, hardware later)
│   ├── style.css                 # shared dark theme
│   ├── overlap.html              # Tool 1: frequency overlap visualizer
│   ├── calculator.html           # Tool 2: induced current calculator
│   ├── harmonics.html            # Tool 3: harmonic stack analyzer
│   ├── exposure.html             # Tool 4: personal exposure estimator
│   ├── compare.html              # Tool 5: 50Hz vs 60Hz country comparison
│   ├── blindspot.html            # Tool 6: the EEG blind spot explainer
│   ├── hum.html                  # Tool 7: 55Hz vagal hum generator
│   └── monitor.html              # Tool 8: live microphone FFT
└── ...
```

---

## SHARED DESIGN (style.css)

| Token | Value | Usage |
|-------|-------|-------|
| background | `#0a0a0f` | page background |
| accent_grid | `#DCA030` | amber — power/grid elements |
| accent_brain | `#00C8DC` | cyan — neural elements |
| accent_overlap | `#CC3333` | red — danger/overlap zones |
| accent_safe | `#33CC66` | green — Schumann/vagal |
| font | `"Space Mono", monospace` | all text |
| responsive | mobile first | |
| nav | bottom bar linking all 8 tools + index | |

---

## TOOL 1: FREQUENCY OVERLAP (overlap.html)

Canvas spectrum display 0–200 Hz. Brainwave bands as colored regions. Grid frequency as draggable vertical line. Harmonics as dimmer lines. Overlap zones pulse red.

**Controls:**
- Frequency slider 0–200 Hz
- Country toggle 50/60/custom
- Harmonic visibility
- Brainwave band toggles
- Zoom to gamma focus (30–100 Hz)

Click any band for explanation popup. Click overlap zone for "what this means."

---

## TOOL 2: INDUCED CURRENT CALCULATOR (calculator.html)

**Faraday's Law:** `E = A × B × 2πf`, then `I = E / R`

**User inputs:** grid frequency (Hz), magnetic field (µT), tissue loop area (cm²), body resistance (Ω/m).

**Presets:**

| Preset | Field |
|--------|-------|
| 🏠 Home | 0.1 µT |
| 🏢 Office | 0.5 µT |
| 📱 Phone charger | 5 µT |
| 🏭 Factory | 10 µT |
| ⚙️ Motor | 100 µT |
| 🔧 Welder | 1000 µT |
| 🧲 MRI | 1.5 T |

**Output:** induced current in nA, visual bar comparing to neural firing range (1–100 nA), plain English assessment, color coded (green/amber/red/purple).

---

## TOOL 3: HARMONIC ANALYZER (harmonics.html)

Canvas spectrum showing fundamental + harmonics through 13th. Amplitude decreases with harmonic order (1/n model).

- **"Dirty electricity" toggle** adds broadband noise between harmonics (simulates switching supplies, dimmers, LEDs)
- **Brain overlay** shows which harmonics fall in which brainwave band
- **Clean vs dirty** comparison mode

---

## TOOL 4: EXPOSURE ESTIMATOR (exposure.html)

**Questionnaire:** country (50/60 Hz), home type, bed-to-breaker distance, work environment, electronics hours, phone charging proximity.

**Outputs:**
- Estimated 24-hour average µT
- Peak µT
- Hours per exposure tier
- Comparison to ICNIRP/IEEE limits
- Comparison to entrainment threshold
- 24-hour timeline visualization
- Practical recommendations

Tone is always calm. *"Your exposure is well within safety limits. Distance is your friend."*

---

## TOOL 5: WORLD FREQUENCY MAP (compare.html)

Emoji grid world map.
- 🟧 = 50 Hz countries
- 🟥 = 60 Hz countries
- 🟪 = Japan (split)

Click country for frequency, voltage, exposure estimate, distance from 40 Hz gamma.

**Side panel:** population-weighted distribution, average gamma distance by population, Japan anomaly callout.

**Data:** hardcoded JSON of ~50 countries with frequency + voltage + population.

---

## TOOL 6: EEG BLIND SPOT (blindspot.html)

Simulated EEG signal (JS-generated sum of sine waves). Four steps:

1. Show raw signal (all frequencies visible in FFT)
2. Apply notch filter at 50/60 Hz (gap appears)
3. Apply low-pass at 50 Hz (gamma disappears)
4. Show what's MISSING (highlighted in red)

**Toggle:** notch on/off, low-pass on/off, grid noise on/off.

**Two panels:** time domain (wiggly line) and frequency domain (FFT).

**Key insight:** *"The notch filter removes grid noise. It also removes brain activity at that frequency. We cannot tell the difference. This is the blind spot."*

Links to Sapien Labs research.

---

## TOOL 7: 55 Hz HUM GENERATOR (hum.html)

Web Audio API oscillator. Slider: 50–60 Hz with markers at 50 (UK grid), 55 (vagal resonance), 60 (US grid). Waveforms: sine/triangle/custom. Oscilloscope visualization.

**Optional 4:6 breath timer overlay:** hum on 6-count exhale, silence on 4-count inhale. This is a Konomioke session in a browser.

**Display:** *"You are generating X Hz. Your grid is Y Hz. Difference: Z Hz. 55 Hz is your frequency, not the grid's."*

---

## TOOL 8: LIVE ROOM MONITOR (monitor.html)

Web Audio API `getUserMedia` for microphone. Real-time 2048-point FFT. Canvas spectrum with markers at 50 Hz (amber), 60 Hz (red), 55 Hz (cyan), and harmonics (100/120 Hz).

**Display:** peak frequency, amplitude at 50/60 Hz, dominant grid frequency detected, harmonic presence (dirty electricity indicator).

All processing client-side. No audio recorded or transmitted. Clear mic-active indicator. One-click stop.

---

## API STUB (api.js)

```js
const GridBrainAPI = {
  // Phase 1: localStorage
  saveReading(data) { /* → localStorage */ },
  getReadings() { /* → from localStorage */ },
  exportReadings() { /* → download JSON */ },
  clearReadings() { /* → wipe */ },

  // Phase 2 stubs: hardware sensor
  connectSensor() { return { status: "mock" } },
  getLiveReading() { return { frequency: 60, amplitude: 0.1, unit: "µT", source: "mock" } },
  startLogging(interval) { /* mock readings → localStorage */ },
  stopLogging() {},

  // Phase 3 stubs: resonator control
  setCounterFrequency(hz) { return { status: "not_implemented" } },
  getResonatorStatus() { return { status: "not_implemented" } }
}
```

Every tool imports `api.js`. When hardware exists, swap the implementation. Tools don't change.

**Clean separation:** UI ←→ API ←→ hardware.

---

## INDEX PAGE (grid-brain/index.html)

**Header:** "⚡ GRID BRAIN" + "Power Grid vs Your Brain"

**Tool cards** (2 columns desktop, 1 mobile):

| Icon | Tool | Tagline |
|------|------|---------|
| 🔌 | Frequency Overlap | "See where the grid meets your gamma" |
| 📐 | Current Calculator | "How much current does the grid induce in you?" |
| 📡 | Harmonic Analyzer | "What harmonics fill your building?" |
| 🏠 | Exposure Estimator | "What's your 24-hour EMF profile?" |
| 🗺️ | World Frequency Map | "50 Hz or 60 Hz? Depends where you are" |
| 🔍 | The Blind Spot | "Why neuroscience can't see this" |
| 🎵 | 55 Hz Hum Generator | "Your frequency, not the grid's" |
| 🎤 | Live Room Monitor | "What frequency is YOUR room humming at?" |

**Footer:** "Grid Brain is part of Lightning Factory" + link to main site.

**Easter egg:** click ⚡ in header 7 times → shows "510,510"

---

## BUILD ORDER

| Priority | Files | Notes |
|----------|-------|-------|
| p=2 | Directory + index.html + style.css + api.js | skeleton |
| p=3 | overlap.html | the core visualization, everything else references it |
| p=5 | calculator.html + blindspot.html | the science tools |
| p=7 | hum.html + monitor.html | the interactive/audio tools |
| p=11 | harmonics.html + exposure.html | the analysis tools |
| p=13 | compare.html | the world map, most data to hardcode |
| p=17 | Connect api.js to hardware | future phase |

Each tool is standalone. Build in any order. Each works independently. Index links them.

---

## FUTURE HARDWARE

### Phase 2: Sensor

USB/BLE gaussmeter ($20–$50 ESP32 module) feeds live data to `monitor.html`. `api.js` `connectSensor()` returns real BLE connection. `getLiveReading()` returns real µT.

### Phase 3: Resonator

Signal generator + amplifier + coil. Generates counter-frequency to null grid EMF per room.

**Workflow:** Measure (`monitor.html`) → Calculate (`calculator.html`) → Counter-signal (resonator) → Verify (`monitor.html` shows reduction).

Per-building calibration based on measured harmonic content.

---

> Every tool is one HTML file.
> Every tool works offline.
> The API stub is localStorage now, hardware later.
> The blind spot has been there for 90 years.
> These tools let you see it.
>
> `teslasolar.github.io/lightning-factory/grid-brain/`
