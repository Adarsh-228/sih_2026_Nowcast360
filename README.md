# ⚡ NowCast360

<p align="center">
  <img src="https://img.shields.io/badge/SIH-2026-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Problem-NowCast360-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Theme-Disaster%20Management-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Flutter-3.x-02569B?style=for-the-badge&logo=flutter" />
  <img src="https://img.shields.io/badge/Mission%20Mausam-Aligned-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-40%25%20Completed%20(Validation)-yellow?style=for-the-badge" />
</p>

> **NowCast360** is an AI-powered convective-scale nowcasting platform that fuses Doppler radar, INSAT satellite imagery and ground-based lightning data to forecast **thunderstorms, hail, downburst winds and cloudbursts up to 6 hours ahead** at 1–3 km spatial resolution.

---

## 📋 Problem Statement & Solution

| Field | Detail |
|---|---|
| **Platform** | NowCast360 |
| **Team** | Debuggers_26 (Smart India Hackathon 2026) |
| **Organisation** | India Meteorological Department (IMD) / NCMRWF / MoES |
| **Lead Time** | 0–6 Hours (Addresses the gap where coarse-grid NWP models lose skill) |
| **Resolution** | 1–3 km Hyper-local GIS Grid |
| **National Priority** | Mission Mausam (2024–2026, ₹2,000 Crore investment) |

### 🎯 4 Severe-Storm Hazards Predicted Core:
1. ⚡ **Thunderstorm & Lightning Strike Density** — strikes/km², CAPE energy, flash rate
2. 🌨️ **Hail Probability & Diameter** — % risk, hail size (mm), Vertically Integrated Liquid (VIL)
3. 💨 **Downburst Wind Velocity** — peak gust velocity (km/h), microburst detection, wind shear
4. 🌧️ **Cloudburst Rainfall Thresholds** — extreme precipitation rate (mm/h), flash flood triggers

---

## 🌟 Why NowCast360 Stands Out

- **Multi-Sensor Data Fusion Engine**: Combines Doppler Weather Radar (DWR) reflectivity & radial velocity fields (~10 min), INSAT-3D/3DR satellite thermal/IR bands (15–30 min), ground-based lightning detection (LLDN, event-based), and NWP background (WRF/GFS, 6–12 hr) into a unified multi-channel tensor.
- **Convective Initiation (CI) Detector**: Flags early storm formation signatures 30–60 minutes *before* a radar cell fully matures (>35 dBZ).
- **Live GIS Hazard Dashboard with Live Countdown Clocks**: Shows hazard zones at 1–3 km resolution with live countdown clocks for storm arrival at specific locations (a usable *"time to impact"*, not just a static probability number).
- **0–6 Hour Lead Time**: Fills the critical forecasting vacuum where coarse NWP models cannot resolve localized severe convection.
- **Multi-Channel Alert Dissemination**: Direct broadcast to local administrations (DDMA/SDMA), aviation sector (aerodrome warnings), power grid operators, and rural farming communities via local language SMS (Hindi, Odia, Bengali).

---

## 📱 Cross-Platform Application (Flutter 3.x)

This repository contains the cross-platform application for mobile and desktop/web dashboard operations.

### Key Screens

| Screen | Description |
|---|---|
| 🏠 **Home** | Real-time GIS radar sweep, live storm arrival countdown clocks, 4-hazard parameter matrix, and data ingestion streaming status |
| 🗺️ **Risk Map** | Interactive GIS map with 0–6 hour forecast horizon slider (`+30m` to `+6h`), 1–3 km grid, hazard filters (Hail, Downburst, Cloudburst, Lightning), and district ranking table |
| 📊 **Analytics** | Multi-sensor charts: 4-hazard lead time evolution curves, Doppler reflectivity (dBZ), INSAT cloud top brightness temp, and Mission Mausam model validation targets (POD ≥ 80%, FAR ≤ 30%, CSI > 0.50) |
| 🔔 **Alerts** | Mobile alert feed with filter pills (`ALL`, `EMERGENCY`, `WARNING`, `WATCH`), CAPE %, MHSS score, regional language SMS preview, and ACK workflows |
| ⚙️ **Shift Ops** | Stakeholder switcher (IMD Forecaster, District Authority, Aviation, Rural Farmer, Power Grid), alert rule thresholds, and Kafka streaming architecture monitors |

---

## 🏗️ Technical Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ATMOSPHERIC DATA SOURCES                            │
│  Doppler Radar (~10 min) · INSAT Satellite (15-30 min) · Lightning (LLDN)  │
│                           NWP Model (6-12 hr)                              │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │ Adapters
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    KAFKA STREAMING BUS & PREPROCESSING                      │
│   Data Validation → Time Alignment → Spatial Reprojection → Common Grid    │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │ Multi-Channel Tensor
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                             AI/ML SERVICES                                  │
│  • ConvLSTM + U-Net: Storm formation, movement, 0–6h multi-hazard risk     │
│  • XGBoost + MHSS: Multi-Hazard Severity Score (1–3 km resolution)         │
│  • Alert Rule Engine: Threshold + Confidence Check · MLflow Registry        │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │ WebSocket / REST API
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    FASTAPI GATEWAY & ALERT DISSEMINATION                    │
│   TimescaleDB · PostgreSQL Master-Slave · E2E Encryption                   │
│   → Forecaster GIS Dashboard · Mobile App (Flutter) · NDMA CAP · SMS Gateway│
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

- **Frontend**: Flutter 3.x (Dart), `flutter_map` + `latlong2`, `fl_chart`, Custom Dark/Grey UI Theme
- **Ingestion & Streaming**: Apache Kafka, Python Adapters
- **Storage**: TimescaleDB (Time-series radar/satellite), PostgreSQL Master-Slave (User & alert metadata)
- **AI/ML Engine**: PyTorch, ConvLSTM, U-Net, XGBoost, Scikit-learn, MLflow
- **Backend API**: FastAPI (REST + WebSocket), Docker Containers

---

## 🚀 Running the Project

```bash
# 1. Install dependencies
flutter pub get

# 2. Run static analysis
flutter analyze

# 3. Run application
flutter run
```

---

## 👥 Team Debuggers_26
Developed for **Smart India Hackathon 2026** under the Disaster Management Theme.  
Aligned with **Mission Mausam (2024–2026)**, Ministry of Earth Sciences (MoES), Govt. of India.
#   s i h _ 2 0 2 6 _ N o e w c a s t 3 6 0  
 