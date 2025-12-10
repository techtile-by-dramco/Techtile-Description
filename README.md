# Techtile Description Repository

This repository is the single source of truth for all structural, geometric, hardware, timing, and calibration metadata of the Techtile R&D infrastructure.  
It contains machine-readable YAML descriptions used across experiments, simulators, [DSS](https://arxiv.org/abs/2311.02662) documentation, beamforming/WPT scripts, acoustic processing, and rover-based localization.

The content here enables full reconstruction, visualization, and validation of Techtile’s physical and logical configuration at any moment in time.

---

# 1. Repository Structure

```
Techtile-Description/
├─ README.md
├─ dss/
│   ├─ testbeds/
│   │   └─ techtile.yaml
│   ├─ data_sources/
│   │   ├─ usrp_b210.yaml
│   │   └─ ni_pxi_daq.yaml
│   ├─ hardware/
│   │   ├─ antennas/
│   │   ├─ microphones/
│   │   ├─ timing/
│   │   └─ cables/
│   └─ environments/
│       └─ techtile_room.yaml
│
├─ geometry/
│   ├─ techtile_room.yml
│   ├─ techtile_tiles.yml
│   ├─ techtile_antenna_locations.yml
│   ├─ techtile_microphone_locations.yml
│   └─ techtile_octoclock_locations.yml
│
├─ calibration/
│   ├─ timing/
│   │   └─ techtile_octoclock_network.yml
│   ├─ rf/
│   ├─ acoustics/
│   │   └─ techtile_acoustic_properties.yml
│   └─ phases/
│       └─ phase_offsets.yml
│
├─ scenes/
│
└─ tools/
    ├─ python/
    │   ├─ visualize_layout.py
    │   ├─ check_layout_against_dss.py
    │   └─ export_tiles.py
    └─ notebooks/
```

---

# 2. Geometry Overview

## 2.1 Global Room Geometry

Defined in `geometry/techtile_room.yml`.

- Room size: **8.0 m × 4.0 m × 2.4 m**
- Coordinate system:
  - Origin at **south-west floor corner**
  - x-axis along room length (8 m)
  - y-axis along width (4 m)
  - z-axis upward (0–2.4 m)
- Surfaces include:
  - floor, ceiling, north/east/south/west walls
- Materials:
  - WikiHouse-style plywood structure
  - 140 detachable tiles of **1.2 m × 0.6 m**

---

## 2.2 Tile Grid Geometry

Defined in `geometry/techtile_tiles.yml`.

- All tiles A01–G20 mapped to:
  - center_position (x,y,z)
  - orientation (normal + Euler angles)
  - surface: wall, floor or ceiling
  - capabilities (RF, microphone, ultrasonic, VLC)
- Tile size: **1.2 m × 0.6 m**
- Tiles are aligned on a deterministic grid.

---

## 2.3 Antenna & Microphone Geometry

### Antennas  
`geometry/techtile_antenna_locations.yml`

Includes SDR channels, tile mapping, position and direction vectors.

### Microphones  
`geometry/techtile_microphone_locations.yml`

Includes DAQ channels, tile mapping, and positions.

---

## 2.4 Octoclock Geometry

`geometry/techtile_octoclock_locations.yml`  
Contains physical coordinates for timing hardware in the room.

---

# 3. Timing Network

## 3.1 Timing Hardware (DSS)

Located in `dss/hardware/timing/`.

Includes:

- external GPSDO 10MHz + PPS
- OctoClock-G hardware descriptors
- timing cables

## 3.2 Logical Timing (L0–L3)

Defined in `calibration/timing/techtile_octoclock_network.yml`.

Describes:

- L0: External time source → TNR0XXX  
- L1: TNR0XXX → TNR01XX / TNR02XX / TNR03XX  
- L2: Fan-out to TNR011X … TNR037X  
- L3: Each tile → Octoclock output mapping  

Enables full timing lineage tracing and debugging.

---

# 4. Calibration Data

## 4.1 Phase Calibration

`calibration/phases/phase_offsets.yml`

Includes:

- Measured per-tile phase offsets
- Reference tile (e.g., G20 = 0°)
- Power = −33 dBm
- Gain = 80 dB
- Waveform amplitude = 0.8
- Measurement notes

## 4.2 Acoustic Calibration

`calibration/acoustics/techtile_acoustic_properties.yml`

Includes:

- RT60 (125 Hz – 8 kHz)
- Ambient noise maps
- Ultrasonic interference sources

---

# 5. DSS Testbed Description

The file `dss/testbeds/techtile.yaml` integrates:

- data sources (USRPs, DAQ)
- hardware components (antennas, microphones, timing, cables)
- channel geometry
- full room and tile geometry
- timing network

This creates a fully self-describing digital representation of Techtile.

---

# 6. Recommended Extensions

## 6.1 Scenes

Add predefined experiment layouts in `scenes/`.

## 6.2 Measurement Sessions

Store reproducible logs in:

```
calibration/sessions/<date>/<session>.yml
```

## 6.3 Digital Twin Export

Add GLTF/OBJ export scripts.

## 6.4 Per-Tile Capability Tables

Extend tiles with RF/acoustic/VLC/backscatter capability flags.

---

# 7. Goals

This repository ensures Techtile is:

- **Reproducible**  
- **Traceable**  
- **Automated**  
- **Documented**  
- **Extendable**  
