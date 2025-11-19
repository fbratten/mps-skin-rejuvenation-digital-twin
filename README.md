# MPS Skin Rejuvenation Digital Twin (Simulated)

**A zero-dependency, in-browser simulator of microphysiological skin–bone marrow co-culture experiments, including RL optimization, Bayesian Optimization, Thompson Sampling, Louvain clustering, DNAm/BioAge simulation, and AI-derived analysis — all written in vanilla JavaScript.**

---

## Overview

This repository provides a **single-page web application** that models a microphysiological system (MPS) for **systemic rejuvenation of human skin**, inspired by:

> Ritter et al., *Systemic factors in young human serum influence in vitro responses of human skin and bone marrow-derived blood cells in a microphysiological co-culture system* (Aging, 2024).

⚠️ **Note:**  
This is an **educational, exploratory simulation**.  
It **does not** reproduce the authors’ proprietary pipelines, datasets, or epigenetic clocks.  
It is **not** a medical device and must not be used for clinical or therapeutic decisions.

---

## Features

### 1. Virtual Wet-Lab Simulator
Simulates an organ-on-chip system including:
- 3D **skin** compartment  
- **Bone marrow** (BM) compartment  
- Recirculating **“serum”** with tunable conditions  

Generates synthetic biomarker responses:
- Ki67-like proliferation  
- Procollagen production  
- BioAge / DNAm-like age estimate  
- Hyaluronic acid & mitochondrial potential  

---

### 2. AI Optimization Suite (Vanilla JS Only)

All algorithms implemented manually, without external libraries:

- **Reinforcement Learning (ε-greedy bandit)**  
- **Bayesian Optimization** with:
  - Gaussian Process (RBF kernel)
  - Expected Improvement (EI) acquisition  
- **Thompson Sampling** (Beta-Bernoulli variant)

Includes full trace visualization on `<canvas>`.

---

### 3. Network & Bioinformatics Tools
- Protein interaction graph (canvas-based)
- **Custom Louvain clustering**
- **Phase-2 aggregated graph** view
- DNAm/BioAge estimation models:
  - OLS / Ridge / Lasso-style regression  
  - Permutation-based feature attribution  

---

### 4. Data Handling
- 100% **client-side**  
- No backend, server calls, cookies, or tracking  
- Generate synthetic experiments  
- Export **JSON** (CSV optional if enabled)  
- In-memory session data (no IndexedDB yet)

---

## Tech Stack

- **HTML5**, **CSS3**, **JavaScript (ES6+)**
- No dependencies, no build step  
- All visuals rendered via **native `<canvas>`**

The project is delivered as a single HTML SPA:

```
src/mps-skin-rejuvenation.html
```

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-user>/mps-skin-rejuvenation-digital-twin.git
cd mps-skin-rejuvenation-digital-twin
````

### 2. Run locally (no build required)

#### Option A — open directly in your browser

Just open:

```
src/mps-skin-rejuvenation.html
```

---

## Usage

### 1. Select a tab

* **Experiment Lab**
* **Optimization Suite (RL / BO / TS)**
* **Proteins & Networks**
* **AI Models**
* **Methylation / BioAge**
* **Data**

### 2. Configure experiment parameters:

* Young vs. old serum
* BM co-culture
* Serum concentration
* Flow
* Culture duration

### 3. Run simulations

* Click **Run experiment**
* Watch metrics & charts update live

### 4. Export data

Go to the **Data** tab and click:

* **Generate dataset**
* **Export JSON** (and CSV if enabled)

---

## Architecture (High-Level)

### Single-file SPA

* HTML structure
* CSS variables & theme
* JavaScript logic embedded in `<script>` blocks

### Event-driven UI

* Tabs toggle visible panels
* No framework, no router

### Simulation Core

* `runExperimentOnce()` builds each synthetic experiment
* RL, BO, GP, TS, and clustering coded manually in plain JS

### Graph & Network System

* Canvas-based layout & rendering
* Louvain detection
* Phase-2 aggregation:

  * `drawAggregatedGraph()`
  * `aggCanvas`

---

## Roadmap

### Phase 1 — Modularization

Move logic out of the HTML file:

```
/src/js/simulation.js
/src/js/optimization.js
/src/js/network.js
```

Possible migration: **TypeScript + Vite**.

---

### Phase 2 — Real Data & Persistence

* Add IndexedDB persistence
* Support CSV upload for:

  * DNAm clocks
  * Biomarkers
* Map dataset columns into the existing AI models

---

### Phase 3 — Advanced Visualization

* Use Plotly (optional) for heatmaps & 3D surfaces
* Add parameter landscapes:
  `(flow, serum%) → rejuvenation score`
* Add PDF export/reporting

---

### Phase 4 — Optional Backend

* Python/FastAPI service for:

* scikit-learn epigenetic clocks
* Heavier optimization tasks
* SPA remains frontend, backend adds ML API

See `ROADMAP.md` for the expanded checklist.

---

## Research Context & Attribution

This work is **inspired by** but **not affiliated with**:

> Ritter et al., *Systemic factors in young human serum influence in vitro responses of human skin and bone marrow-derived blood cells in a microphysiological co-culture system*, Aging, 17(7):1784–1809.

* Uses **synthetic** models and data
* No original datasets, pipelines, or proprietary models
* Refer to the publication for real scientific findings
