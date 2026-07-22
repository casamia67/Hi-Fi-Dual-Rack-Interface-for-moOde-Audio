# Hi-Fi Dual-Rack Interface for moOde Audio

A modular, dual-frame vertical Hi-Fi Rack wrapper designed for **moOde Audio Player** on Raspberry Pi. Engineered specifically for dedicated tablet displays (e.g., using Fully Kiosk Browser) and touchscreen kiosks.

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Platform](https://img.shields.io/badge/Platform-moOde%20Audio-blue)
![Stack](https://img.shields.io/badge/Stack-HTML5%20%7C%20CSS3%20%7C%20Vanilla%20JS-orange)

---

## 🎯 What It Does

This project transforms a standard tablet or touch display into a physical-style Hi-Fi component rack that integrates two independent web applications into a single view:

1. **Top Rack Unit (40% height)**: Embeds the official moOde Audio native player (`index.php`), giving you direct access to playback controls, volume, album art, and library management.
2. **Bottom Rack Unit (60% height)**: Embeds an active, real-time VU Meter visualizer (`vumeter.html`).
3. **On-Demand Fullscreen Expansion**: Provides dedicated overlay buttons for both modules. Tapping a button instantly expands that specific unit to cover 100% of the screen (ideal for adjusting VU settings or browsing deep music libraries) without reloading the page or interrupting audio playback.

---

## ⚙️ How It Works (Technical Architecture)

The system is built as a zero-dependency, lightweight web wrapper (`rack_master.html`) using modern front-end techniques:

### 1. Isolated Component Sandboxing (iFrames)
* Instead of modifying moOde's core codebase or merging complex JavaScript libraries, the layout uses two HTML5 `<iframe>` elements.
* This architectural choice guarantees **complete CSS and JavaScript isolation**. Scripts and WebSocket connections running inside moOde (`index.php`) and the VU Meter (`vumeter.html`) execute in separate execution contexts, preventing state corruption or style pollution.

### 2. Proportional Flexbox Layout
* The outer container (`.hifi-rack`) uses CSS Flexbox in vertical direction (`flex-direction: column`) with `100vh` / `100vw` viewport boundary caps.
* Height distribution is governed by CSS `flex` ratios (`flex: 35` for top, `flex: 65` for bottom), ensuring smooth scaling across diverse tablet screen aspect ratios (16:9, 16:10, 4:3) while preserving Hi-Fi component aesthetics.

### 3. Non-Blocking DOM State Expansion
* When you tap an expansion toggle button, a lightweight Vanilla JavaScript function (`toggleMoodeExpansion()` or `toggleVUExpansion()`) dynamically applies an `.expanded` class to the target container.
* The `.expanded` CSS rule leverages fixed positioning (`position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; z-index: 9999;`).
* **Key advantage**: Because the iFrame element is re-styled in-place within the DOM tree rather than re-created or reloaded, all WebSocket streams, active canvas animations, and player states remain **100% continuous and uninterrupted**.

### 4. Non-Interfering Control Placement
* Control buttons use `backdrop-filter: blur()` overlays with distinct anchor points:
  * Top Rack Button: Anchored at **bottom-right** (`bottom: 10px; right: 10px`) to prevent overlapping moOde's top navigation bar.
  * Bottom Rack Button: Anchored at **top-right** (`top: 10px; right: 10px`) within the VU Meter viewport.

---

## 📁 File Structure

Place `rack_master.html` alongside your native moOde files in the webroot directory (`/var/www/html/`):

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

You are free to use, modify, distribute, and integrate this software into open-source or commercial projects with minimal restriction.

💡 Credits & Acknowledgments
moOde Audio Player: Designed to complement the open-source audio ecosystem developed by Tim Curtis and contributors (moodeaudio.org).

Developed with AI assistance and collaborative engineering support via Gemini AI.

```text
/var/www/
        ├── rack_master.html            # Main Hi-Fi Rack master wrapper
        ├── index.php                   # Native moOde Audio player
        └── vumeter/
                   └──index.html        # Dynamic VU Meter module

