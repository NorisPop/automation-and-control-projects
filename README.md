# Automation & Control Engineering Projects

A collection of academic and industrial automation projects covering **system identification** in MATLAB/Simulink and **PLC-based control and monitoring** using Siemens LOGO! 8.3.

## Overview

| Folder | Project | Main Technologies |
| --- | --- | --- |
| `system-identification-matlab/` | System Identification of Analog & Power Electronics Circuits | MATLAB, Simulink, System Identification Toolbox |
| `paint-booth-ventilation-plc/` | Paint Booth Ventilation Automation | Siemens LOGO! 8.3, SINAMICS V20 VFD, Spring-Return Damper |
| `compressed-air-monitoring-plc/` | Compressed Air Consumption Monitoring & Reporting | Siemens LOGO! 8.3 Web Server, IO-Link, CSV / Excel |

---

## 1. System Identification (`system-identification-matlab/`)

Four-part identification study performed in MATLAB/Simulink on two physical electrical systems:

- **Sallen-Key Low-Pass Filter (Step Response & Linear Regression):** Identified an underdamped second-order transfer function ($K = 3.5086$) by extracting transient peaks and applying logarithmic linear regression, validated in state-space form ($\text{eMPN} = 12.57\%$).
- **Sallen-Key Low-Pass Filter (Chirp Signal):** Estimated gain, damping ratio, and natural frequency from frequency-sweep excitation and resonance peak analysis ($\text{eMPN} = 13.21\%$ in the filtering zone).
- **DC-DC Boost Converter (PRBS / SPAB Calibration):** Designed a composite trapezoidal + Pseudo-Random Binary Sequence (PRBS) excitation signal calibrated to the converter's rise time and fastest dynamics.
- **Parametric Identification & Refinement:** Compared polynomial and state-space structures (`ARX`, `ARMAX`, `IV4`, `OE`, `BJ`, `SSEST`, `N4SID`, `PEM`) using detrending, decimation, median filtering, windowing, and cross-validation. `ARMAX` achieved the best overall balance (`72.02%` fit), followed by `BJ` (`68.36%`) and `SSEST` (`67.52%`).

**Contents:** Full technical report (`PDF`).

---

## 2. Paint Booth Ventilation Automation (`paint-booth-ventilation-plc/`)

Hardware specification and wiring design for a paint booth exhaust and fresh-air ventilation system controlled by a **Siemens LOGO! 8.3** PLC (`230 VAC`, `8 DI`, `4 DO` relay):

- **Safety Interlock:** Fans are permitted to start only after open-position confirmation is received from the intake damper.
- **Intake Damper:** Belimo/Lufberg `DA10S220S` spring-return actuator (`230 VAC`, `10 Nm`, `~90 s` travel) with `2 × SPDT` auxiliary contacts for open/closed feedback.
- **Dual-Fan Drive Architecture:**
  - **Fan V1:** Single-phase motor (`230 VAC`, `50 Hz`) controlled ON/OFF via an `AC-3` contactor.
  - **Fan V2:** Asynchronous motor driven by a **Siemens SINAMICS V20** VFD (`6SL3210-5BE21-1UV0`, `1.1 kW`) configured in Fixed Speed Binary mode (`0 / 30 / 38 / 50 Hz` presets) with fault feedback to the PLC.
- **Control Panel:** START/STOP buttons, speed preset selector, alarm reset, and status LEDs.

**Contents:** Wiring guide and hardware identification sheet (`PDF`).

---

## 3. Compressed Air Consumption Monitoring & Reporting (`compressed-air-monitoring-plc/`)

Automated monitoring and reporting system for compressed air usage based on an **IO-Link flowmeter** and a **Siemens LOGO! 8.3** PLC with a custom **Web Server**:

- **Real-Time Web Interface:** Password-protected dashboard displaying instantaneous flow rate ($\text{m}^3/\text{h}$), current-day consumption ($\text{m}^3$, counted from `00:00`), and previous-day consumption ($\text{m}^3$, archived daily at `00:00`).
- **Excessive-Flow Alarm:** Visual indicator triggered automatically if instantaneous flow exceeds the configured threshold for more than `10 s`, with automatic reset once flow returns to normal.
- **Data Logging & Reporting:** One-minute interval logging exported via `.csv` from the PLC web server and processed in Microsoft Excel for weekly and monthly consumption tracking.
- **Counter Management:** Automatic daily reset at `00:00` (with manual recovery handled via LOGO! Soft Comfort V8 / IO-Link after power loss).

**Contents:** LOGO! Soft Comfort network project (`.wnp`), operator user manual (`PDF`), and Excel reporting template (`.xlsx`).

---

## Repository Structure

```text
other-projects/
├── README.md
├── system-identification-matlab/
│   └── system-identification-report.pdf
├── paint-booth-ventilation-plc/
│   ├── wiring-guide.pdf
│   └── hardware-specification.pdf
└── compressed-air-monitoring-plc/
    ├── logo-web-server.wnp
    ├── user-manual.pdf
    └── consumption-report-template.xlsx
