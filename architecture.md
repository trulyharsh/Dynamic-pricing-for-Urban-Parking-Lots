# 🧱 Project Architecture & Workflow

## 📌 Overview

This system simulates real-time pricing for urban parking lots using live sensor data and adapts dynamically based on lot conditions like occupancy, traffic, and queue length. The architecture is built using:

- **Pathway** for real-time stream processing  
- **Pandas** for input transformation  
- **Bokeh** for interactive visualization

---

## 🛠 Architecture Components

| Component | Role |
|----------|------|
| **CSV Dataset (`dataset.csv`)** | Simulates real-time sensor feeds per parking lot |
| **Pathway Input Table** | Ingests CSV as a live stream (row-by-row) |
| **Pricing UDF** | Computes pricing using Model 1 (occupancy-based) or Model 2 (demand-based) |
| **Output Table** | Streamed output of final prices with timestamps |
| **Output CSV (`pathway_output.csv`)** | Stores computed prices for downstream analysis |
| **Bokeh Visualizations** | Plots price evolution, feature-response graphs |

---

## 🔄 Workflow Steps

### 🔹 Step 1: Data Ingestion (Simulated Streaming)

- The CSV file (`dataset.csv`) contains live-like parking lot updates.
- We read the file using Pathway in `streaming` or `static` mode.
- Pathway automatically commits each batch using `autocommit_duration_ms`.

### 🔹 Step 2: Pricing Model Selection

Two pricing models are supported:

- **Model 1: Linear Pricing**  
  Price increases with occupancy ratio using a linear formula.

- **Model 2: Demand-Based Pricing**  
  Price depends on multiple demand signals:
  - Occupancy Ratio
  - Queue Length
  - Traffic Score
  - Special Day Indicator
  - Vehicle Type Weight

Pricing logic is embedded inside a `@pw.udf` (Pathway UDF).

### 🔹 Step 3: Real-Time Processing in Pathway

- Each row is processed by the `pricing_udf` in real-time.
- The selected model is passed via a configuration flag (`model_choice`).
- Pathway emits results continuously to an output table.

### 🔹 Step 4: Stream Output to CSV

- Final pricing with timestamps and lot IDs is written to `pathway_output.csv`.
- This file captures the stream’s output for later plotting and analysis.

### 🔹 Step 5: Visualization Using Bokeh

- We load the output CSV using Pandas
- Use Bokeh to create:
  - Real-time line plots of price per lot
  - Comparison between lots (as simulated competitors)
  - Scatter plots (e.g., price vs occupancy/traffic) to justify model behavior

### 🔹 Step 6: Reporting & Evaluation

- Behavior is evaluated visually and numerically.
- Pricing is shown to be smooth, bounded, and explainable.
- Project outputs include:
  - Working notebook
  - PDF report (optional)
  - GitHub documentation and visual evidence

---

## ✅ Why This Architecture Works

- **Real-Time Ready**: Pathway simulates live input and scales to real-time apps.
- **Explainable ML**: Models are transparent with interpretable formulas.
- **Interactive Insights**: Bokeh provides clear, browser-based analytics.
- **Modular Logic**: You can easily add Model 3 (competitive pricing) or UI in the future.

---

## 📈 Architecture Flow Summary

1. **Input Stream** → 2. **Feature Extraction** → 3. **Pricing Logic** →  
4. **Output Stream** → 5. **CSV File** → 6. **Bokeh Visualization**
