# SSS Freespan Detector

[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-blue.svg)](https://microsoft.com/windows)
[![Release](https://img.shields.io/badge/Release-Standalone%20Executable-green.svg)]()
[![Inference Engine](https://img.shields.io/badge/Inference-ONNX%20Runtime%20CPU-orange.svg)]()
[![Compatible Format](https://img.shields.io/badge/Compatible-SonarWiz%20CSF%20%2F%20DB3-informational.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**SSS Freespan Detector** is a specialized, high-performance desktop application designed for marine geophysicists, hydrographic surveyors, and subsea pipeline inspection engineers. It combines a **lightweight ONNX Runtime inference client** with **calibrated acoustic physics ray-tracing** to automatically detect, measure, annotate, and export subsea pipeline free spans directly from **Sidescan Sonar (SSS)** survey lines in native Chesapeake Sonar Format (`.csf`).

> **Built with:** ONNX Runtime, OpenCV, NumPy, PyProj, Pillow, and Matplotlib.

---

## 🚀 Key Features

- **Direct SonarWiz & CSF File Processing**:
  - Open SonarWiz project folders directly and seamlessly navigate through survey lines (`Ctrl+Left` / `Ctrl+Right`).
  - Native parsing of Chesapeake Sonar Format (`.csf`) files, extracting acoustic ping waterfalls, ping coordinates, towfish altitude, and navigation headers.
  - Automatic Coordinate Reference System (CRS) detection and georeferencing via `pyproj`.

- **Flexible ONNX Inference Engine**:
  - Zero-configuration, CPU-optimized execution powered by Microsoft ONNX Runtime—**no GPU, CUDA, or PyTorch installation required**.
  - Compatible with any standard object detection model exported to ONNX format (such as YOLO-architecture models).
  - Configurable confidence and Non-Maximum Suppression (NMS) thresholds.

- **Calibrated Acoustic Slant-Range Physics**:
  - Automated calculation of free span **clearance height ($H$)** and **span length ($L$)** derived from acoustic shadow geometry and slant-range parameters:
    $$\text{Span Height } (H) = \frac{L_{\text{shadow}} \times H_{\text{towfish}}}{R_{\text{slant}}}$$
  - Instantaneous, zero-lag bounding box manipulation (physics ray-tracing computed dynamically upon export or peak snapping).

- **Interactive Waterfall Sonar Viewer**:
  - High-performance, multi-resolution waterfall canvas with smooth panning and zooming.
  - Auto-scroll capability with adjustable playback speed for continuous line review.
  - Customizable display palettes (Grayscale, Copper, and Inverted).
  - Starboard canonical mirroring for standardized neural network inference.

- **Precision 8-Handle Box Annotation**:
  - Interactive bounding box editing with 8 resize handles.
  - **Acoustic Shadow Peak Snapping (`P`)**: One-key algorithmic alignment to the exact acoustic shadow boundary.
  - Quick box confirmation (`C`) and deletion (`Delete` / `Backspace`).

- **Dual Export Workflows**:
  1. **SonarWiz Project Contact Database Export (`Ctrl+E`)**:
     - Automatically updates/creates SonarWiz contact database files: `ContactDBR001.db3` and `FeatureData01.cdf`.
     - Generates georeferenced high-resolution contact image snapshots in `Targets/`.
     - Produces comprehensive inspection CSV reports (`fs_report.csv`) with KP, Easting, Northing, Span Height, and Span Length.
  2. **YOLO Dataset Export (`Ctrl+S`)**:
     - Generates 640×640 canonical acoustic tiles with normalized bounding box annotations (`<class> <x> <y> <w> <h>`).
     - Automatically partitions datasets into `train` and `val` splits with a generated `data.yaml` for active learning and model retraining.

- **Integrated SonarWiz Database Manager**:
  - Built-in utility to inspect, sync, verify, and vacuum SonarWiz SQLite databases (`.db3`).

---

## 📁 Repository Contents

```text
SSS Freespan Detector/
├── SSS Freespan Detector.exe   # Standalone 64-bit Windows application
├── _internal/                  # Bundled runtimes, libraries, and dependencies
├── .gitattributes              # Git text normalization attributes
├── LICENSE                     # MIT License
└── README.md                   # Application documentation
```

---

## 🧠 Model Weights Setup

The application runs inference using a standalone **ONNX Runtime** engine and requires an ONNX model file to execute automated detections.

### Loading Model Weights:
1. Obtain or train a freespan detection model exported to ONNX format (e.g., `best.onnx`).
2. You can either:
   - Place the `best.onnx` file directly into this folder next to `SSS Freespan Detector.exe`, **OR**
   - In the application window, click the **Browse** button next to the **Model** path field to select your custom `.onnx` file from any directory.

---

## ⚡ Quick Start

### 1. Requirements
- **Operating System**: Windows 10 or Windows 11 (64-bit).
- **Prerequisites**: None. All dependencies (Python runtime, ONNX Runtime, OpenCV, Pillow, PyProj) are bundled within `_internal/`.

### 2. Launching the Application
Double-click `SSS Freespan Detector.exe` to launch the application.

### 3. Step-by-Step Workflow
1. **Load SonarWiz Project / CSF Files**:
   - Click **Browse Project** (`Ctrl+O`) to select your SonarWiz project directory containing `.csf` line files.
   - Select a survey line from the left sidebar file list.
2. **Inspect the Waterfall**:
   - Use the mouse wheel to scroll through the acoustic waterfall.
   - Click and drag to pan or select regions of interest.
   - Adjust colormap (Grayscale / Copper) or contrast/gain if needed.
3. **Run AI Free Span Detection**:
   - Ensure a valid `.onnx` model is loaded in the Model field.
   - Press **`F5`** or click **Run AI Detection** in the control panel.
   - The detector scans the active survey line and highlights detected free spans with bounding boxes and estimated dimensions.
4. **Review & Fine-Tune**:
   - Click any bounding box to select it.
   - Drag handles to resize or reposition.
   - Press **`P`** to automatically snap the boundary to the peak acoustic shadow.
   - Press **`C`** to confirm the annotation.
5. **Export Results**:
   - Press **`Ctrl+E`** to export contacts into your SonarWiz project (`ContactDBR001.db3`, `FeatureData01.cdf`, `Targets/`, and `fs_report.csv`).
   - Press **`Ctrl+S`** to export annotated tiles in standard YOLO format for model fine-tuning.

---

## ⌨️ Keyboard Shortcuts

| Shortcut | Description |
| :--- | :--- |
| `Ctrl + O` | Open / Browse SonarWiz Project Folder |
| `F5` | Run AI Detection on active CSF line |
| `P` | Snap selected box to peak acoustic shadow boundary |
| `C` | Confirm selected detection box |
| `Delete` / `Backspace` | Delete selected box |
| `Ctrl + Left Arrow` | Navigate to Previous Survey Line |
| `Ctrl + Right Arrow` | Navigate to Next Survey Line |
| `Ctrl + E` | Export to SonarWiz Project Contact Database (`.db3`, `.cdf`, `Targets/`, `CSV`) |
| `Ctrl + S` | Export to YOLO Dataset (`data.yaml`, images, labels) |
| `Esc` | Stop active auto-scroll |
| `Mouse Wheel` | Scroll waterfall view vertically |
| `Left Click + Drag` | Pan waterfall view or draw/resize bounding boxes |
| `Right Click` | Open context menu / Delete box |

---

## 🔬 Acoustic Free Span Calculation Principles

Sidescan Sonar identifies free spans through two distinct acoustic signatures:
1. **Pipeline Seabed Detachment**: The pipe reflection curves away from the seabed contact line.
2. **Acoustic Shadow Formation**: The gap between the pipe bottom and the seabed creates an acoustic shadow whose geometry is proportional to the elevation:

$$H = \frac{L_{\text{shadow}} \times H_{\text{towfish}}}{R_{\text{slant}}}$$

Where:
- $H$: Computed free span clearance height (meters).
- $L_{\text{shadow}}$: Shadow length measured in acoustic slant-range (meters).
- $H_{\text{towfish}}$: Altitude of the sidescan towfish above seabed (meters).
- $R_{\text{slant}}$: Slant-range distance from towfish to pipe reflection (meters).

The application computes these parameters directly using the sensor altitude recorded in each CSF ping header.

---

## 📄 License

This software and its distribution are licensed under the **[MIT License](LICENSE)**.

You are free to use, distribute, and integrate this application in personal, academic, and commercial survey operations with no warranty or liability.

---

## 👤 Author & Support

- **Author**: Rizky Affianto
- **Contact**: [rizkyaffianto@gmail.com](mailto:rizkyaffianto@gmail.com)
- **Application**: SSS Freespan Detector
- **Copyright**: © 2026 Rizky Affianto. All rights reserved.
