#  Weather Trend Forecasting & Global Health Risk Analysis

> **PM Accelerator Mission:**
> *"By making industry-leading tools and education available to individuals
> from all backgrounds, we level the playing field for future PM leaders.
> This is the PM Accelerator motto, as we grant aspiring and experienced
> PMs what they need most – Access. We introduce you to industry leaders,
> surround you with the right PM ecosystem, and discover the new world of
> AI product management skills."*

---

##  Project Overview

This project analyzes the **Global Weather Repository** dataset to investigate
a central public health question:

> **"Can daily weather patterns forecast dangerous air quality — and what
> does the health risk burden look like across cities at opposite ends of
> the climate spectrum?"**

Using two years of daily weather and air quality data across 257 cities
worldwide, this project combines time series forecasting, anomaly detection,
feature importance analysis, and spatial visualization to build a
data-driven picture of global health risk from combined heat and
pollution exposure.

---

## 📂 Repository Structure
Weather-Analysis/
│
├── weather.ipynb                 # Main analysis notebook
├── cleaned_weather_data.csv      # Cleaned dataset (19 complete cities)
├── global_health_burden_map.html # Interactive world map
├── requirements.txt              # Python dependencies
├── .gitignore
└── README.md

---

## 📊 Dataset

- **Source:** [Global Weather Repository — Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code)
- **Size:** 142,289 rows, 40+ features
- **Coverage:** 257 cities worldwide
- **Time Range:** May 2024 — May 2026
- **Frequency:** Daily readings per city

---

## 🔬 Methodology

### 1. Exploratory Data Analysis
- Identified 257 unique cities with daily weather and air quality readings
- Confirmed 2-year time range with daily frequency
- Identified 19 cities with complete 732-day records for time series analysis
- Constructed composite burden score combining normalized temperature
  and AQI to select 4 focus cities

### 2. Focus Cities Selected
| City | Country | Burden Profile |
|---|---|---|
| N'djamena | Chad | High burden — extreme heat + poor AQI |
| Baghdad | Iraq | High burden — desert heat + pollution |
| Bern | Switzerland | Low burden — temperate + clean air |
| Kyiv | Ukraine | Low burden — cool + relatively clean |

### 3. Climate Analysis — Seasonal Decomposition
- Decomposed temperature, AQI, and humidity for all 4 cities
- Found strong but irregular seasonality in high-burden cities
- Identified anomalous residual step changes in N'djamena and Baghdad
  around January 2025 — flagged for anomaly detection

### 4. Time Series Forecasting
Three models built and compared for AQI forecasting:

| Model | N'djamena | Baghdad | Bern | Kyiv |
|---|---|---|---|---|
| ARIMA MAPE | 37.07% | 28.60% | 15.85% | 35.93% |
| Prophet MAPE | 43.09% | 38.73% | 23.46% | 45.96% |
| Ensemble MAPE | 23.23% | 32.55% | 17.89% | 29.58% |

ARIMA consistently outperformed Prophet, consistent with weak seasonal
patterns found during decomposition. The weighted ensemble improved
accuracy for volatile high-burden cities — N'djamena and Kyiv —
where individual models struggled most.

### 5. Anomaly Detection — Isolation Forest
- Applied multivariate Isolation Forest across 8 features:
  temperature, humidity, and 6 pollutant variables
- Contamination set at 5% — approximately 37 anomalous days per city
- **Key finding:** PM2.5 levels were 2-3x higher on anomalous days
  across all cities. Temperature showed little elevation — pollution
  spikes occur independently of heat

### 6. Feature Importance — XGBoost
- XGBoost regressor trained to predict AQI from weather and
  pollutant variables
- **Key finding:** PM2.5 accounted for 93-99% of feature importance
  across all four cities — confirming anomaly detection findings

### 7. Spatial Analysis
- Interactive world map of all 257 cities colored by combined
  burden score
- Regional burden ranking across 6 continents:

| Region | Avg Burden Score |
|---|---|
| Middle East | 0.627 |
| Asia | 0.548 |
| Africa | 0.438 |
| Americas | 0.253 |
| Oceania | 0.233 |
| Europe | 0.215 |

---

##  Key Findings

1. **PM2.5 is the dominant driver of dangerous air quality** — accounting
   for 93-99% of AQI prediction importance across all cities and climate
   types, confirmed independently by both XGBoost and anomaly detection

2. **Dangerous air quality days are not primarily heat-driven** — anomalous
   days showed 2-3x higher PM2.5 but minimal temperature elevation,
   meaning pollution spikes occur independently of weather patterns

3. **High-burden cities have less forecastable AQI** — N'djamena and
   Baghdad showed MAPE of 37% and 29% respectively, while Bern achieved
   16%. Cities where early warning systems are most needed are hardest
   to forecast

4. **Ensemble forecasting helps most where it matters most** — weighted
   ensemble improved accuracy for volatile high-burden cities but not
   for stable low-burden cities

5. **Global health risk follows a clear geographical gradient** — Middle
   Eastern and Asian cities carry disproportionate combined heat and
   pollution burden, nearly 3x higher than European cities

---

##  How To Run

### Prerequisites
```bash
pip install -r requirements.txt
```

### Steps
1. Download the dataset from
   [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code)
   and place it in the project root
2. Open `weather.ipynb` in Jupyter Notebook or VS Code
3. Run all cells sequentially from top to bottom
4. Interactive map will be saved as `global_health_burden_map.html`

### To View Interactive Map
Open `global_health_burden_map.html` directly in any web browser —
no Python required

---

## 🛠️ Libraries Used

| Library | Purpose |
|---|---|
| Pandas, NumPy | Data manipulation |
| Matplotlib | Static visualizations |
| Plotly | Interactive geographical map |
| Statsmodels | Seasonal decomposition, ARIMA |
| pmdarima | auto_arima parameter selection |
| Prophet | Bayesian time series forecasting |
| Scikit-learn | Isolation Forest, preprocessing |
| XGBoost | Feature importance analysis |

---

## 👤 Author

**Ankita Kundu**
B.Tech Information Technology — Institute of Engineering and Management, Kolkata
[GitHub](https://github.com/Elixer05/Weather-Analysis)

---

## 📎 Submission Note

This project was completed as part of the PM Accelerator
Data Science Technical Assessment, 2026.
