# Research Context

This simulator is inspired by a published microphysiological co-culture study on **systemic factors in young human serum and their effect on human skin and bone marrow-derived cells** (MPS “organ-on-chip” platform).

> Ritter J., Falckenhayn C., Qi M., et al. *Systemic factors in young human serum influence in vitro responses of human skin and bone marrow-derived blood cells in a microphysiological co-culture system.* Aging (Albany NY). 2024; 17(7):1784–1809.

This repository is **not** affiliated with, endorsed by, or a re-implementation of the original authors’ analysis pipeline. It is an educational, research-adjacent tool that mimics the structure and qualitative findings of the paper using synthetic data.

---

## 1. Core Biological Ideas (Qualitative)

From the source study (see the paper for full details):

- A microphysiological system (MPS) combines:
  - 3D human skin models
  - A bone marrow (BM) niche
  - Recirculating human serum (young vs old)
- Key observations (simplified):
  - **Young serum + BM co-culture** is associated with:
    - Higher proliferation (e.g. Ki67)
    - Improved extracellular matrix markers (e.g. procollagen)
    - Lower biological/epigenetic age signatures
    - Improved mitochondrial and metabolic readouts
  - **Skin-only** or **old serum** conditions show weaker or opposite effects.
  - A set of **secreted proteins** and **network hubs** are enriched in rejuvenating conditions.

For exact metrics, effect sizes and statistics, always consult the original article.

---

## 2. How the Simulator Relates to the Study

The app encodes these ideas in a deliberately simplified way:

- The **Experiment Lab** tab offers:
  - Serum age: `young`, `old`, `mixed`
  - BM co-culture: `yes/no`
  - Serum concentration, flow and culture duration
- `runExperimentOnce(conf)` produces synthetic outputs:
  - Ki67, Procollagen, Hyaluronic Acid, Mito potential, Biological Age
  - A derived “rejuvenation score” combining these metrics. 

The rules are chosen so that:

- **Young + BM** tends to:
  - Increase Ki67, Procollagen, HA, Mito
  - Decrease biological age
- **Young without BM** shows only mild effects.
- **Old serum** tends to reduce regenerative markers and/or increase biological age.

The **protein and network** features seed the network with hubs named in the paper, but use synthetic connectivity and edge weights.

---

## 3. What Would Be Needed for Real Experimental Use

To move from a qualitative toy to a lab-adjacent companion:

1. **Calibration against real data**
   - Fit the parameters in `runExperimentOnce` to real experiment outputs.
   - Align the rejuvenation score with actual composite endpoints from the lab (Ki67, ECM markers, epigenetic clocks, etc.).

2. **Real epigenetic clocks**
   - Replace the synthetic DNAm generator with:
     - Real CpG beta values.
     - Validated epigenetic clock implementations (Horvath, Hannum, GrimAge, etc.) licensed and implemented correctly.
   - Validate clock predictions against known age standards.

3. **Uncertainty and statistics**
   - Provide confidence intervals and uncertainty estimates for all predictions.
   - Implement proper statistical tests (power analysis, multiple testing correction where applicable).

4. **Reproducibility and provenance**
   - Persist:
     - Raw data
     - Derived metrics
     - Parameter settings
     - Application version (Git commit)
   - Export machine-readable logs that can be archived alongside wet-lab experiment records.

5. **Validation and peer review**
   - Compare simulator outputs to held-out experimental data.
   - Document any discrepancies and limitations openly.
   - Treat this tool as an **exploratory aid**, not as a decision system, unless fully validated.

---

## 4. Usage & Ethics

- This simulator is currently suitable for:
  - Teaching and demonstrations.
  - Exploring optimization strategies conceptually.
  - Prototyping user interfaces and workflows for future lab software.

- It is **not** suitable for:
  - Clinical or diagnostic decision-making.
  - Regulatory submissions.
  - Patient-level predictions.

If you extend this tool and use actual human data:

- Ensure you have appropriate **consent** and **approvals**.
- De-identify datasets before upload.
- Document your data governance and security posture in `SECURITY.md`.

---

## 5. Citation

If you use this repository in academic work, please:

1. Cite the original paper for experimental details.
2. Cite this codebase (see `CITATION.cff`) as the source of the simulation and UI.
