To transition this from a "Demo" to a "Research Tool," the following features are critical:

### 1. Feature: "Experiment Persistence Layer"
* **Description:** Move data storage from volatile RAM (`DATA = []`) to a persistent local database (IndexedDB) or a lightweight backend.
* **The "Why":** Currently, if the user refreshes the page, their 40-round optimization run is lost. Scientific workflows require historical logging and reproducibility.
* **Tech Implementation:**
    * *Client-side:* Implement `Dexie.js` (wrapper for IndexedDB) to store experiment runs locally within the browser.
    * *Architecture:* Decouple the `runExperimentOnce` function so it writes to the DB, and the UI subscribes to DB changes.

### 2. Feature: "Real-World Data Injection"
* **Description:** Allow the "AI Models" tab to train on uploaded CSVs of *actual* CpG methylation data, rather than the current `synthDNAm` function which generates sine-wave noise.
* **The "Why":** The current math is mathematically correct (Matrix operations are valid) but applied to random noise. Allowing real data input makes the tool usable for actual preliminary analysis.
* **Tech Implementation:**
    * Parse uploaded CSVs (using a library like `PapaParse`).
    * Map dynamic columns to the `X` matrix used in the existing `solve(XT_X, XT_y)` linear algebra function.

### 3. Feature: "Interactive Sensitivity Analysis (Heatmaps)"
* **Description:** Replace the 2D bar charts with 3D surface plots or 2D heatmaps showing the relationship between two variables (e.g., `Flow Rate` vs. `Serum Conc`) and the `Rejuvenation Score`.
* **The "Why":** Biological optimization is rarely univariate. Researchers need to see the "landscape" of optimization to identify local minima/maxima, which is the core value of the Bayesian Optimization feature already present.
* **Tech Implementation:**
    * Replace the custom Canvas drawing with a visualization library like **Plotly.js**.
    * Run a grid search in the background (Web Worker) to populate the heatmap data without freezing the UI.

## 4. Strategic Roadmap

| Phase | Objective | Key Deliverables | Est. Complexity (Fibonacci) |
| :--- | :--- | :--- | :--- |
| **1** | **Modularization** | Port single HTML file to React/Vite environment. Break `solve()`, `louvain()`, and `runExperiment()` into dedicated utility services. | 8 |
| **2** | **Persistence & I/O** | Implement IndexedDB storage. Add "Project" management (load/save workspaces). Replace synthetic data generators with CSV parsers for real data input. | 5 |
| **3** | **Advanced Viz** | Replace raw Canvas rendering with Plotly/Recharts. Implement the Heatmap/Surface plot for BO visualization. Add PDF report generation. | 8 |
| **4** | **Backend Integration** | (Optional) Connect to a Python/FastAPI backend to run heavy ML tasks (Scikit-learn) instead of JS-based linear algebra. | 13 |

## 5. Immediate Next Steps

1.  **Refactor to Typescript:** The math functions (`solve`, `rbf`, `louvain`) are complex and untyped. Moving to TypeScript is urgent to ensure the mathematical operations are type-safe (e.g., ensuring matrix dimensions match).
2.  **Extract the Logic:** Copy the math logic (everything inside the `<script>` tag from line 400 onwards) into a separate `simulation.js` file.
3.  **Unit Test the Math:** Verify the `solve` (Gaussian elimination) function against known outputs to ensure the "AI" is accurate, as floating-point errors in JS can derail these algorithms.
4.  **User Action:** Generate the **TypeScript interfaces** for the data structures (Experiment, Protein, Network Node) to start the refactoring process?
