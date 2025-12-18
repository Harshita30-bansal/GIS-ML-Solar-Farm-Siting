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
