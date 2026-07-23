# ISRO-Optical-Flow-Satellite-Interpolation
<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Orbitron&weight=900&size=36&duration=3000&pause=1000&color=FF6A00&center=true&vCenter=true&width=900&lines=PHOENIX+%F0%9F%94%A5;Filling+the+Blind+Spots+in+India's+Sky" alt="Phoenix " />

<br/>

### *When a cyclone intensifies from Category 1 to Category 3 in 20 minutes — INSAT-3DS sees nothing. We fix that.*

<br/>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)
![AWS](https://img.shields.io/badge/AWS_S3-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=FF9900)
![NetCDF](https://img.shields.io/badge/NetCDF4-.nc%20files-0A0E1A?style=for-the-badge)

<br/>

[![ISRO BAH 2026](https://img.shields.io/badge/ISRO%20BAH-2026-FF6A00?style=for-the-badge)](https://www.isro.gov.in)
[![Problem Statement](https://img.shields.io/badge/PS--12-Fill%20in%20the%20Frames%20Seamlessly-4B6FF6?style=for-the-badge)]()
[![Team](https://img.shields.io/badge/Team-Phoenix-red?style=for-the-badge)]()

<br/>

[🚀 How It Works](#-how-it-works--end-to-end) · [✨ Features](#-what-we-deliver) · [🏗️ Architecture](#-system-architecture) · [💰 Cost](#-implementation-cost) · [👥 Team](#-team-phoenix)

</div>

---

<br/>

## 🛰️ The Problem — India's Satellite Blind Spot

<table>
<tr>
<td width="50%">

### ❌ What Exists Today

**Traditional Optical Flow** *(Farneback, Lucas-Kanade)*
- Assume pixels slide like solid objects
- Break the moment clouds rotate, form, or dissolve
- Produce blurred, ghosted, physically meaningless output

**Generic Video Interpolation** *(RIFE, Super SloMo)*
- Built for cinema slow-motion — not meteorology
- Treat thermal infrared temperature as RGB color pixels
- Single-scale — miss fast cells inside slow systems

</td>
<td width="50%">

### ✅ What Phoenix Builds Instead

**Physics-Aware Satellite Frame Interpolation**
- 🔬 Brightness temperature preserved in Kelvin throughout
- 🌀 Dual-scale RAFT flow: mesoscale + convective simultaneously
- 🎯 Confidence map on every output pixel — forecasters know exactly what to trust
- 🔁 Pretrain on GOES/Himawari → fine-tune natively on INSAT-3DS
- 📊 Cloud Motion Score + Temporal Consistency Score (novel metrics)

</td>
</tr>
</table>

> **One number that tells the whole story:** INSAT-3DS photographs India every **30 minutes**. A cyclone can intensify Category 1 → 3 in **20 minutes**. Flood-triggering storm cells form and collapse in **under 15 minutes**. Disaster response teams are making life-and-death decisions on data that is already half an hour old.

---

<br/>

## 🔥 How It Works — End to End

```
Raw .nc frames (t=0 min, t=30 min)
         │
         ▼
┌─────────────────────────────────────────────┐
│        PHYSICS-AWARE PREPROCESSING          │
│  Percentile clipping · Kelvin preserved     │
│  Per-scene normalisation — no flattening    │
└─────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────┐
│      DUAL-SCALE RAFT OPTICAL FLOW           │
│  Coarse (1/8 res) → cyclone system drift    │
│  Fine   (1/2 res) → convective cell motion  │
│            ↓ Fused flow field               │
└─────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────┐
│     MODIFIED RIFE FRAME SYNTHESIS           │
│  Frame A + Frame B + fused flow → t=15 min  │
│  Loss: L1 + VGG perceptual + Laplacian edge │
└─────────────────────────────────────────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐  ┌──────────────────┐
│Synthetic│  │ CONFIDENCE HEAD  │
│ Frame  │  │Per-pixel 0→1 map │
│(.nc out)│  │Flags forming /   │
└────────┘  │dissipating clouds│
            └──────────────────┘
         │
         ▼
┌─────────────────────────────────────────────┐
│       VALIDATION + LIVE DASHBOARD           │
│  MSE · PSNR · SSIM · FSIM                  │
│  Cloud Motion Score · Temporal Consistency  │
│  Real vs AI timelapse · Confidence overlay  │
└─────────────────────────────────────────────┘
```

---

<br/>

## ✨ What We Deliver

<table>
<tr>
<td width="50%">

### 🌡️ 1. Physics-Preserving Pipeline
Brightness temperature stays in Kelvin throughout. Per-scene percentile normalisation — physical meaning **never** sacrificed for model convenience.

### 🌀 2. Dual-Scale Atmospheric Flow
RAFT backbone running at two resolutions simultaneously:
- **Coarse** → large weather system drift
- **Fine** → fast-moving convective cells
Both fused **before** synthesis.

### 🎯 3. Satellite-Native Synthesis
Modified RIFE with a three-term loss:
- Pixel reconstruction (L1)
- Perceptual similarity (VGG)
- **Laplacian edge loss** — cloud boundaries stay sharp, not blurry

</td>
<td width="50%">

### 🗺️ 4. Per-Pixel Confidence Maps
Every synthetic frame ships with a reliability heatmap. Regions where clouds are forming or dissipating are automatically flagged **low-confidence**. Meteorologists know exactly what to trust — making this operationally deployable, not just a demo.

### 🔁 5. Pretrain → Fine-tune Transfer
Learns atmospheric motion patterns on GOES-19 & Himawari-8 (10-min real ground truth). Fine-tunes specifically on INSAT-3DS sensor characteristics — so INSAT data **feels native**, not foreign.

### 📈 6. Metrics-Rich Dashboard
MSE · PSNR · SSIM · FSIM + two novel domain-specific metrics:
- **Cloud Motion Score** — tracks cluster centroid trajectories
- **Temporal Consistency Score** — measures sequence smoothness

</td>
</tr>
</table>

---

<br/>

## 🏗️ System Architecture

```
╔══════════════════════════════════════════════════════════╗
║                    DATA SOURCES                          ║
║  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐    ║
║  │   GOES-19    │ │  INSAT-3DS   │ │  Himawari-8  │    ║
║  │ NOAA AWS S3  │ │   MOSDAC     │ │   JMA AWS    │    ║
║  │   10 min     │ │   30 min     │ │   10 min     │    ║
║  └──────┬───────┘ └──────┬───────┘ └──────┬───────┘    ║
║         └────────────────┼─────────────────┘            ║
╚══════════════════════════╪═════════════════════════════╝
                           ▼
╔══════════════════════════════════════════════════════════╗
║               PREPROCESSING ENGINE                      ║
║      NetCDF parser · Kelvin normaliser                  ║
║      Frame aligner · Pair sampler                       ║
╚══════════════════════════╪═════════════════════════════╝
                           ▼
╔═══════════════════════════════════════════════════════╗
║  ┌ ─ ─ ─ ─ ─ ─ ─  AI CORE  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┐   ║
║  │                                               │   ║
║  │  ┌─────────────────┐    ┌──────────────────┐  │   ║
║  │  │ RAFT Flow       │───▶│  Modified RIFE   │  │   ║
║  │  │ Estimator       │    │  + Confidence    │  │   ║
║  │  │ Coarse 1/8 res  │    │    Head          │  │   ║
║  │  │ Fine   1/2 res  │    └──────────────────┘  │   ║
║  │  └─────────────────┘                          │   ║
║  │  ┌───────────────────────────────────────┐    │   ║
║  │  │     Transfer Learning Controller      │    │   ║
║  │  │  Pretrain (GOES/Himawari) → Fine-tune │    │   ║
║  └ ─┤         (INSAT-3DS sensor)            ├─ ─┘   ║
║     └───────────────────────────────────────┘        ║
╚══════════════════════════╪═════════════════════════════╝
                           ▼
╔══════════════════════════════════════════════════════════╗
║                   OUTPUT LAYER                          ║
║  ┌──────────────────────┐   ┌──────────────────────┐   ║
║  │    Web Dashboard     │◀─▶│   Output .nc Store   │   ║
║  │ Timelapse · Metrics  │   │ Synthetic frames +   │   ║
║  │ Confidence heatmap   │   │      metadata        │   ║
║  └──────────────────────┘   └──────────────────────┘   ║
╚══════════════════════════════════════════════════════════╝
```

---

<br/>

## 🖥️ Dashboard — What the Meteorologist Sees

```
┌──────────────────────────────────────────────────────────────────────┐
│  PHOENIX  Live  Compare  Metrics  About          Event: Bay of Bengal│
├───────────────┬────────────────────────┬─────────────────────────────┤
│  FILTERS      │  t = 00:00  Real       │  t = 00:15 ← AI  Synthetic  │
│               │  ┌──────────────────┐  │  ┌──────────────────────┐   │
│  Satellite:   │  │                  │  │  │                      │   │
│  INSAT-3DS    │  │  ☁️  cloud blob   │  │  │   ☁️  AI-interpolated │   │
│               │  │                  │  │  │   cloud position     │   │
│  30→15 min    │  └──────────────────┘  │  └──────────────────────┘   │
├───────────────┼────────────────────────┴─────────────────────────────┤
│  [green zone] │     SSIM: 0.91   PSNR: 34.2 dB   Cloud MS: 0.87     │
│ High confidence│  ████████████████████████████████░░░  0.93 / 1.00   │
│  [amber zone] │  Temporal consistency                                 │
│ Low confidence│                                                       │
├───────────────┴───────────────────────────────────────────────────────┤
│  [00:00] [00:15 ★] [00:30] [00:45 ★] [01:00]    ★ = AI-synthesized  │
└───────────────────────────────────────────────────────────────────────┘
```

---

<br/>

## 🧬 Technology Stack

```
DATA LAYER ─────────────────────────────────────────────────────────
  netCDF4          xarray           NumPy           Boto3
  file parsing     array ops        normalisation   AWS S3 access
  Sources: NOAA GOES-19 AWS S3 · MOSDAC INSAT-3DS · JMA Himawari-8

AI CORE ────────────────────────────────────────────────────────────
  RAFT                  RIFE (modified)       Confidence Head
  dual-scale flow       frame synthesizer     uncertainty decoder
  Framework: PyTorch · Loss: L1 + VGG perceptual + Laplacian edge

TRAINING ───────────────────────────────────────────────────────────
  PyTorch Loop          W&B Tracking          Colab / Kaggle GPU
  training loop         experiment tracking   free GPU compute

OUTPUT ─────────────────────────────────────────────────────────────
  React.js         Leaflet.js        Plotly           Flask API
  dashboard UI     geo overlays      interactive viz  REST endpoints

EVALUATION ─────────────────────────────────────────────────────────
  scikit-image     SSIM · PSNR · MSE · FSIM    Cloud Motion Score
  standard metrics                              Temporal Consistency
```

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=flat-square&logo=plotly&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=FF9900)
![Kaggle](https://img.shields.io/badge/Kaggle-20BEFF?style=flat-square&logo=kaggle&logoColor=white)

---

<br/>

## 💰 Implementation Cost

<table>
<tr>
<td width="50%">

| Phase | What's Needed | Estimated Cost |
|-------|--------------|----------------|
| **Prototype** | Colab/Kaggle free GPU, GOES-19 subset fine-tuning | **₹0 – ₹3,000** |
| **Pilot (3 months)** | Extended training runs, dashboard hosting | **₹15,000 – ₹30,000** |
| **Operational/month** | Cloud GPU inference, dashboard serving | **₹5,000 – ₹10,000** |

> All satellite data — GOES-19, INSAT-3DS, Himawari-8 — is **publicly available at zero cost**. All frameworks are **open source**. Zero licensing cost.

</td>
<td width="50%">

<div align="center">

### 💡 The ROI That Matters

A new **geostationary satellite** costs

## ₹400–600 crore

to build and launch.

Our solution delivers **equivalent temporal resolution improvement** at

## 0.001% the cost

*~₹5,000–10,000/month in cloud GPU inference*

</div>

</td>
</tr>
</table>

---

<br/>

## 🗺️ Roadmap

```
Phase 1 — Prototype (Hackathon)              [In Progress]
  ✅ Physics-aware data pipeline (.nc → tensor)
  ✅ RAFT dual-scale flow estimator
  ✅ Modified RIFE with Laplacian edge loss
  ✅ Confidence head output
  ✅ React dashboard with SSIM/PSNR/FSIM metrics

Phase 2 — Validation (Post-hackathon)        [Planned]
  🔄 Full GOES-19 training dataset (6 months)
  🔄 Cyclone event benchmark tests
  🔄 Cloud Motion Score + Temporal Consistency scoring
  🔄 INSAT-3DS fine-tuning and sensor alignment

Phase 3 — Operational Deployment             [Vision]
  💡 Real-time pipeline integration with MOSDAC feeds
  💡 IMD meteorologist dashboard rollout
  💡 Extension to 7.5-minute resolution (recursive interpolation)
  💡 Generalisation to Meteosat-12, MSG, and future INSAT missions
  💡 Flood and fire rapid-detection alert module
```

---

<br/>

## 🔬 What Makes This Different

<table>
<tr>
<td align="center" width="33%">

### 🌐 Built for Satellites<br/>Not Cinema

Every other approach forces video models onto satellite data. Phoenix is designed from scratch for thermal infrared physics — from normalisation to loss function to flow scale.

</td>
<td align="center" width="33%">

### 🌀 Two Scales,<br/>Fused

The **first approach** to explicitly model mesoscale drift and convective-scale motion **simultaneously** — directly addressing the failure mode of every existing method.

</td>
<td align="center" width="33%">

### 🗺️ Confidence Maps<br/>That Operate

A frame without a reliability score is a demo. Per-pixel confidence turns Phoenix into something **IMD could actually deploy** — giving forecasters region-by-region reliability alongside every generated image.

</td>
</tr>
</table>

> *"We don't interpolate pixels — we model atmospheric physics at multiple scales, synthesize physically consistent intermediate frames, and tell you exactly how much to trust each one."*

---

<br/>

## 📊 Expected Validation Metrics

| Metric | Method | Target |
|--------|--------|--------|
| **SSIM** | Structural similarity vs real GOES-19 hidden frame | > 0.88 |
| **PSNR** | Peak signal-to-noise ratio (dB) | > 32 dB |
| **MSE** | Mean squared error on brightness temperature | < 2.0 K² |
| **FSIM** | Feature similarity index | > 0.90 |
| **Cloud Motion Score** *(novel)* | Centroid trajectory accuracy | > 0.85 |
| **Temporal Consistency** *(novel)* | Sequence smoothness across generated frames | > 0.92 |

*Validation strategy: hide every third GOES-19 frame, predict it, compare to reality. Self-supervised — no manual labels required.*

---

<br/>

## 📡 Data Sources

| Satellite | Source | Cadence | Channel | Access |
|-----------|--------|---------|---------|--------|
| **GOES-19** | NOAA AWS S3 `s3://noaa-goes19/` | 10 min | ABI Channel 13 (10.3 µm) | Free, public |
| **INSAT-3DS** | MOSDAC portal (ISRO) | 30 min | TIR1 (~10.8 µm) | Free, registration |
| **Himawari-8** | JMA AWS mirror | 10 min | Band 13 (10.4 µm) | Free, public |

*All input/output files are `.nc` (NetCDF) or `.h5` (HDF5) format, preserving full geospatial metadata, calibration coefficients, and brightness temperature units.*

---

<br/>

## 👥 Team Phoenix

<div align="center">

| Role | Name |
|------|------|
| 🚀 **Team Leader** | Anushka Dabhade |
| ⚙️ **Member** | Krithik Naidu |
| 🎨 **Member** | Deval Thakor |
| 📊 **Member** | Fgaun Soni |

</div>

---

<br/>

<div align="center">

### 🏆 Built for ISRO Bharatiya Antariksh Hackathon 2026

**Problem Statement 12 — Fill in the Frames Seamlessly**

*Enhancing Temporal Resolution of Satellite Imagery using AI/ML-based Optical Flow*

<br/>

![Made with love](https://img.shields.io/badge/Made%20with-🔥%20by%20Phoenix-FF6A00?style=for-the-badge)
![ISRO](https://img.shields.io/badge/For-ISRO%20BAH%202026-4B6FF6?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

<br/>

*"2–4× more satellite 'looks' at a cyclone, flood, or wildfire — using the data we already have."*

</div>
