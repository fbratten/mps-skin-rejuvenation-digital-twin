# Architecture & Extensibility Guide

This document explains how the current prototype is structured and what another developer or researcher would need to do to turn it into a **lab-adjacent research tool** rather than just an advanced demo. 

---

## 1. High-Level Overview

- **Frontend:** Vanilla JavaScript (ES6+), HTML5, CSS3 (CSS Variables for theming).
- **Dependencies:** None – no NPM packages, no bundler.
- **Runtime:** Everything runs in the browser; there is no backend service.
- **Storage:** In-memory `DATA` array with optional JSON/CSV export.

The entire app currently lives in a **single HTML file**:

- UI, styling and logic are combined in `src/mps-skin-rejuvenation.html`.
- Sections in the UI are toggled using `hidden` attributes (simple SPA routing).
- All math (Gaussian Processes, Louvain clustering, regression, CV) is implemented manually in JS.

---

## 2. Main Functional Modules (in the script)

Even though everything is in one file, you can conceptually split it into modules:

### 2.1 Simulation Core

- `runExperimentOnce(conf)`  
  Encodes a skin + bone marrow co-culture system with:
  - Serum age: **young / old / mixed**
  - BM co-culture: **yes / no**
  - Serum concentration, culture duration, flow
- Returns synthetic biomarker deltas for:
  - Ki67, Procollagen, Hyaluronic Acid, Mitochondrial potential
  - Biological age (lower is “younger”)

- `calculateScore(result)`  
  Produces a scalar “rejuvenation score” from the biomarker outputs.

**Extension points:**

- Replace synthetic logic with calibrated functions fit to real experimental data.
- Make all scaling factors configurable (JSON config instead of hard-coded constants).

---

### 2.2 Optimization Suite

Three optimizers all call `runExperimentOnce` under different sampling policies:

- **RL (ε-greedy multi-armed bandit)**  
  - Explores serum concentration arms; decaying ε.
  - Tracks empirical value estimates `Q[a]` and chooses best.

- **Bayesian Optimization (Gaussian Process + EI)**  
  - RBF kernel with adjustable lengthscale and variance.
  - Re-fits GP each iteration and selects next point by Expected Improvement.

- **Thompson Sampling (Beta-Bernoulli approximation)**  
  - Treats each arm as a Beta distribution over “success probability”.
  - Updates `α/β` from a success threshold on the rejuvenation score.

**Extension points:**

- Generalize the domain from a single scalar (serum %) to multivariate (serum %, flow, days).
- Allow optimizer configuration via UI (priors, exploration hyper-parameters).
- Log every optimization step to a persistent store for reproducibility.

---

### 2.3 DNAm Clock & AutoML-Lite

- `synthDNAm(n, p)`  
  Generates synthetic CpG-like features linked to chronological age.

- `zscore`, `linfit`, `solve`  
  Provide normalization and linear regression via Gaussian elimination.

- Clock training:
  - OLS, Ridge, Lasso regression on the synthetic dataset.
  - `Permutation importance` to show feature relevance.
  - `k-fold CV` to compare model families.

**Extension points:**

- Replace `synthDNAm` with a pipeline that accepts **real methylation CSVs**.
- Separate model training from visualization, so real epigenetic clocks can be plugged in (e.g. via backend API).

---

### 2.4 Protein Network & Louvain Clustering

- Builds a synthetic protein interaction network seeded with hubs (e.g. MMP9, CD163, APOB, CRP, SERPINE1).
- Implements **Louvain clustering**:
  - `modularity(comm, nodes, edges)` to evaluate Q.
  - `louvainPhase1` for local node moves.
  - `louvainPhase2` for community aggregation.
- Visualizes:
  - Phase-1 node-level communities.
  - Phase-2 aggregated “super-nodes” (community centroids) on a separate canvas.

**Extension points:**

- Load real PPI edges from a file or API.
- Allow exporting and importing community assignments for downstream analysis.

---

### 2.5 Data Table, Export & Stats

- Global `DATA` array stores experiment runs.
- UI can:
  - Generate synthetic batches.
  - Export/import JSON.
  - Export CSV.
- Basic stats:
  - Compares Young+BM vs Old+BM for Ki67 and BioAge differences.
  - Computes simple t-like statistics.

**Extension points:**

- Replace `DATA` with a **persistence layer** (IndexedDB or backend DB).
- Add experiment IDs, timestamps, operator, notes, and parameter metadata.
- Add full provenance: code version (Git SHA), optimizer settings, etc.

---

## 3. What Others Need to Do to Make This a “Real Lab Tool”

This is the hand-off list for someone who wants to take this from prototype to lab-ready companion:

1. **Modularize the Codebase**
   - Split the single HTML into:
     - `src/simulation.js`
     - `src/optimization.js`
     - `src/dnam.js`
     - `src/network.js`
     - `src/ui.js`
   - Introduce a build system (Vite, ESBuild, etc.) and optionally TypeScript.

2. **Persistence & Reproducibility**
   - Introduce a **storage layer**:
     - Option A: IndexedDB (Dexie.js) for local-only use.
     - Option B: Backend (e.g. FastAPI + Postgres) for shared use.
   - Log every run with:
     - Input parameters
     - Output metrics
     - Optimizer used and hyper-parameters
     - Application version / Git commit

3. **Real Data Integration**
   - Allow upload of:
     - Real biomarker tables (Ki67, Procollagen, etc.)
     - Real DNAm CSVs.
   - Provide a mapping UI so users can match their column names to the expected fields.

4. **Validation & QA**
   - Add unit tests around:
     - `solve`, GP kernel, EI, Louvain modularity.
   - Add regression tests to ensure refactors do not change scientific outputs unexpectedly.

5. **Advanced Visualization**
   - Replace hand-drawn canvas charts with:
     - Heatmaps and surface plots (e.g. Plotly.js).
   - Provide “experiment report” exports (PDF/HTML) summarizing parameters, plots and conclusions.

6. **Security, Ethics & Compliance**
   - If real human data is used:
     - Enforce de-identification.
     - Document consent expectations and data retention.
   - If a backend is added:
     - Add authentication, authorization and audit logs.
     - Document threat model and security practices in `SECURITY.md`.

---

## 4. Roadmap Snapshot

For more detail, see `ROADMAP.md`, but at a glance:

1. **Phase 1 – Modularization & TypeScript**
2. **Phase 2 – Persistence & Upload of Real Data**
3. **Phase 3 – Advanced Visualization & Reporting**
4. **Phase 4 – Optional Python Backend for heavy ML**

Each phase should ship as a small, testable milestone so the tool can already be useful in exploratory settings before full production hardening.
