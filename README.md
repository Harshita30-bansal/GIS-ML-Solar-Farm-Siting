# 🌞 GIS–Machine Learning Framework for Optimal Solar Farm Siting

## 📌 Project Overview
This project develops an integrated **GIS–Machine Learning decision-support framework** to identify and rank **optimal utility-scale solar farm locations in Bengaluru district**.  
By combining **spatial analysis**, **Multi-Criteria Decision Analysis (MCDA)**, and **machine learning–based solar irradiance forecasting**, the framework enables **scientific, data-driven solar planning**.

The final output is a **Solar Farm Suitability Map** highlighting high-potential zones that are:
- Environmentally safe
- Technically feasible
- Economically viable

---

## 🎯 Problem Statement
With rapidly growing energy demand and increasing stress on Bengaluru’s power infrastructure, there is a strong need to expand **clean and renewable energy sources**.  
Although Bengaluru has high solar potential, selecting suitable land for **large-scale solar farms** is challenging due to:
- Urbanization
- Environmental constraints
- Terrain limitations
- Infrastructure accessibility

This project addresses this challenge by integrating **GIS layers with machine learning–based solar energy prediction** to support informed decision-making.

---

## 🧩 Objectives
- Acquire, preprocess, and integrate GIS thematic layers (terrain, land use, infrastructure, environment).
- Develop and validate a **Machine Learning model (XGBoost)** to predict solar irradiance.
- Apply **Multi-Criteria Decision Analysis (MCDA)** using weighted spatial factors.
- Generate a **Solar Farm Suitability Map** identifying optimal zones for large-scale solar development.

---

## 🗺️ Study Area
- **Region:** Bengaluru Urban & Rural Districts, Karnataka, India  
- **CRS:** EPSG:32643 (WGS 84 / UTM Zone 43N)

---

## 🧪 Datasets Used
- **Solar Irradiance & Weather:** NASA POWER (2016–2025)
- **Land Use / Land Cover:** ArcGIS Living Atlas (10 m)
- **Elevation (DEM):** NRSC Bhoonidhi (30 m)
- **Infrastructure (Roads, Powerlines, Substations):** OpenStreetMap (via Geofabrik & QuickOSM)
- **Administrative Boundaries:** GADM
- **Protected Areas:** Environmental Justice Atlas

---

## ⚙️ Methodology

### 1️⃣ Data Collection & Preparation
- All GIS layers were standardized to a common CRS.
- Layers were clipped to the Bengaluru boundary.
- LULC (10 m) was resampled to 30 m to maintain spatial consistency with DEM.

### 2️⃣ Terrain & Energy Modeling
- Derived **slope** and **aspect** from DEM.
- Computed **annual solar radiation** using the SAGA “Potential Incoming Solar Radiation” model.
- Seasonal variation and terrain shadowing were captured using fine temporal resolution.

### 3️⃣ Proximity Analysis
- Generated distance rasters (30 m resolution) for:
  - Roads
  - Power lines
  - Substations
- Closer proximity was assigned higher suitability.

### 4️⃣ GIS Reclassification & MCDA
All layers were reclassified into **5 suitability classes (1–5)**.
Weights were assigned using **Analytical Hierarchy Process (AHP)**:

| Factor | Weight |
|------|--------|
| Total Solar Energy | 35% |
| Distance to Roads | 15% |
| Distance to Power Lines | 15% |
| Distance to Substations | 10% |
| Slope | 10% |
| Land Use/Land Cover | 10% |
| Aspect | 5% |

### 5️⃣ Machine Learning Model
- **Model:** XGBoost Regression
- **Target Variable:** ALLSKY_SFC_SW_DWN (Solar Irradiance)
- **Features:**  
  - Cyclical time encodings (hour/month sin–cos)  
  - Lag features (1–168 hours)  
  - Rolling window statistics  
  - Meteorological predictors (temperature, humidity, wind, pressure, UV)

- **Split:** 80/20 time-aware train–test split

**Performance:**
- R² = 0.9998  
- RMSE = 4.06  
- MAE = 2.01  

### 6️⃣ Final Site Selection
- Applied suitability threshold (Score > 4.8).
- Converted high-suitability zones into polygons.
- Enforced **minimum contiguous area constraint (>5 hectares)**.
- Ranked sites based on area and mean solar potential.
- Forecasted 2026 solar energy using ML model.

---

## 📊 Results
- Generated a **final Solar Farm Suitability Map** for Bengaluru.
- Identified **Top 3 optimal sites**, each:
  - Larger than 5 hectares
  - Receiving >6.9 kWh/m²/day
- ML-based forecast estimates **average solar potential of 5.98 kWh/m²/day** for Bengaluru.

---

## 💡 Applications & Impact
- **Government:** Scientific zoning for renewable energy planning.
- **Investors:** Reduced prospecting risk and lower feasibility costs.
- **Environment:** Avoids forests, lakes, and protected zones.
- **Energy Planning:** Supports long-term clean energy strategy.

---


## 🔮 Future Work
- Incorporate socio-economic constraints (land price, ownership, population).
- Add grid capacity and substation load limits.
- Deploy as an interactive Web-GIS decision support tool.
- Extend framework to hybrid renewable planning (solar + wind).

---

## 👩‍💻 Team Members
- **IMT2023034** – Nainika Agrawal  
- **IMT2023035** – Harshita Bansal  
- **MT2024011** – Aishwarya J Panampilly  

---

## 🛠️ Tools & Technologies
- QGIS, SAGA GIS
- Python (Pandas, NumPy, XGBoost, Matplotlib)
- MCDA (AHP)
- OpenStreetMap, NASA POWER

## 🔧 How to Set Up and Run the Project
**1️⃣ Software Requirements**

Make sure the following software is installed:

- ✅ QGIS

Download QGIS Long Term Release (LTR)

Link: https://qgis.org/en/site/forusers/download.html

Recommended version: QGIS 3.x LTR

- ✅ Python

Python 3.8 or higher

Download: https://www.python.org/downloads/

**2️⃣ Python Libraries Required**

Install the required Python packages using pip:

pip install pandas numpy matplotlib scikit-learn xgboost


(If you are using Anaconda, you can install via conda install as well.)

**3️⃣ Clone or Download the Repository**
git clone https://github.com/Harshita30-bansal/GIS-ML-Solar-Farm-Siting.git


Or download as ZIP from GitHub and extract it.

**4️⃣ Open the GIS Project in QGIS**

- Open QGIS

- Go to File → Open Project

- Select the file:

Solar GIS.qgz


All GIS layers (LULC, DEM, roads, powerlines, suitability maps) will load automatically.

📌 If any layer path is broken:
Right-click layer → Set Data Source → reconnect from the /data folder.

**5️⃣ Running the GIS Analysis (Optional – for reproduction)**

Inside QGIS:

- Use Raster Calculator for weighted overlay

- Use Processing Toolbox → SAGA → Potential Incoming Solar Radiation

- Use Proximity (Raster Distance) tools for distance to roads/grid

- Apply Reclassification based on defined thresholds

- Convert final raster to polygons and filter areas > 5 hectares

(These steps were already executed; users can view outputs directly.)

**6️⃣ Running the Machine Learning Model**

-Navigate to the ML folder or Python script/notebook

- Ensure NASA POWER CSV data is present

- Run the script or notebook to:

- Preprocess irradiance data

- Train XGBoost model

- Evaluate RMSE, MAE, R²

- Forecast solar energy for 2026

Example:

python train_xgboost_model.py

**7️⃣ Output Files**

The project produces:

- Final Solar Farm Suitability Map

- Ranked optimal solar farm locations

- ML prediction plots

- Forecasted solar irradiance values (kWh/m²/day)

**8️⃣ Notes**

- All spatial analysis is performed at 30m resolution for consistency.

- The ML model uses a time-aware train–test split to avoid data leakage.

- The project is designed as a decision-support framework, not a construction blueprint.
