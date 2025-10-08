
---

# 🚦 Traffic-congestion-prediction

Traffic-congestion prediction in **Vilnius**

---

## 📘 Project Description – End-to-End Solution

This repository provides an **end-to-end multimodal traffic-volume forecasting solution** for **Vilnius city**.
It integrates **real-time API data** and (optionally) **SUMO simulation outputs** to predict several congestion-related KPIs for city streets.

### 🔑 Pipeline Components

1. **Data Collection**

   * **HERE Traffic API v7** – every 15 min (jam-factor, mean speed, ideal-speed, confidence score)
   * **OpenWeather API** – hourly (temperature, wind-speed, precipitation, weather description)
   * **Eismoinfo.lt road sensors** – every 30 min (road-surface temperature, dew-point, friction)
   * **(Optional) SUMO simulation** – synthetic network data (`edge_output.xml`, `emissions.xml`, …)

2. **Data Pre-processing**

   * Cleans missing values and standardises multilingual fields → English
   * Scales all numeric variables to **[0-1]**
   * Builds **sliding-window sequences** for supervised learning (past *n* steps → next-step target)

3. **Model Training & Evaluation**

   * Deep-learning models: **LSTM, Bi-LSTM, CNN-LSTM, Transformer**
   * Metrics: **MAE**, **RMSE**, **R²**
   * Trained on **multivariate time-series** covering traffic, weather, and road-surface data

4. **Prediction Service / Deployment**

   * Accepts the most recent feature window for each road segment
   * Forecasts next-step **jam-factor**, **travel-time**, **emission index** (or other selected KPIs)
   * Can be benchmarked against **SUMO** synthetic scenarios
   
---

## 🎯 Target Variables

| Target                                  | Meaning                                  | Source                                                        |
| --------------------------------------- | ---------------------------------------- | ------------------------------------------------------------- |
| **y₁ – Jam_Factor_Next (0-10)**         | Congestion level at the next 15-min step | HERE API or SUMO-derived: `10 × (1 – meanSpeed/allowedSpeed)` |
| **y₂ – Travel_Time_Next (min/km)**      | Expected travel-time for next step       | HERE API or SUMO `traveltime`                                 |
| **y₃ – Emission_Index_Next (g CO₂/km)** | Environmental impact metric              | SUMO `emissions.xml` or estimated from speed-flow curve       |

(Can be extended with **Speed_Next**, **Accident_Risk**, **Queue_Length_Next**, etc.)

---

## 🟢 (X,y) Examples – Jam Factor (**y₁**)

### Example A – typical rush-hour (Gedimino pr.)

```
X =
[
  [ jam_factor=3.2, veh_speed=24, temp=14.0, wind=3.1, friction=0.87 ],
  [ jam_factor=4.1, veh_speed=21, temp=13.8, wind=3.3, friction=0.86 ],
  [ jam_factor=4.8, veh_speed=17, temp=13.5, wind=3.2, friction=0.83 ],
  [ jam_factor=5.6, veh_speed=12, temp=13.0, wind=3.6, friction=0.80 ]
]

y = [ jam_factor_next = 6.3 ]
```

### Example B – weather deterioration (Vilniaus g.)

```
X =
[
  [ jam_factor=2.0, veh_speed=32, temp=25.0, wind=2.5, precipitation=0.0 ],
  [ jam_factor=2.8, veh_speed=28, temp=24.5, wind=3.2, precipitation=0.0 ],
  [ jam_factor=3.5, veh_speed=22, temp=24.0, wind=5.5, precipitation=1.2 ],
  [ jam_factor=4.7, veh_speed=18, temp=23.2, wind=6.1, precipitation=2.8 ]
]

y = [ jam_factor_next = 5.5 ]
```

### Example C – icy surface (V. Kudirkos a.)

```
X =
[
  [ jam_factor=3.9, veh_speed=20, friction=0.74, road_temp=0.5 ],
  [ jam_factor=4.4, veh_speed=18, friction=0.72, road_temp=0.2 ],
  [ jam_factor=5.0, veh_speed=15, friction=0.70, road_temp=-0.5 ],
  [ jam_factor=5.5, veh_speed=12, friction=0.67, road_temp=-1.0 ]
]

y = [ jam_factor_next = 6.2 ]
```

---

## 🟢 (X,y) Examples – Travel Time (**y₂**)

### Example A – moderate flow (Gedimino pr.)

```
X =
[
  [ jam_factor=3.0, veh_speed=26, length_km=0.45, hour=08, dow=1 ],
  [ jam_factor=3.6, veh_speed=22, length_km=0.45, hour=08, dow=1 ],
  [ jam_factor=4.4, veh_speed=18, length_km=0.45, hour=08, dow=1 ],
  [ jam_factor=5.1, veh_speed=14, length_km=0.45, hour=08, dow=1 ]
]

y = [ travel_time_next_min_per_km = 6.1 ]
```

### Example B – clear off-peak (Labdarių g.)

```
X =
[
  [ jam_factor=1.5, veh_speed=33, length_km=0.32, weather_desc=clear_sky ],
  [ jam_factor=1.7, veh_speed=34, length_km=0.32, weather_desc=clear_sky ],
  [ jam_factor=1.9, veh_speed=32, length_km=0.32, weather_desc=clear_sky ],
  [ jam_factor=2.0, veh_speed=31, length_km=0.32, weather_desc=clear_sky ]
]

y = [ travel_time_next_min_per_km = 2.7 ]
```

### Example C – heavy rain (Vilniaus g.)

```
X =
[
  [ jam_factor=4.8, veh_speed=16, precipitation=3.2, wind=5.4 ],
  [ jam_factor=5.6, veh_speed=12, precipitation=4.0, wind=6.1 ],
  [ jam_factor=6.1, veh_speed=10, precipitation=4.7, wind=6.3 ],
  [ jam_factor=6.5, veh_speed=9,  precipitation=5.0, wind=6.5 ]
]

y = [ travel_time_next_min_per_km = 7.5 ]
```

---

## 🟢 (X,y) Examples – Emission Index (**y₃**)

### Example A – congested urban (Vilniaus g.)

```
X =
[
  [ veh_speed=12, flow=600, density=45, temp=5.0, wind=3.0 ],
  [ veh_speed=11, flow=610, density=47, temp=4.8, wind=3.2 ],
  [ veh_speed=10, flow=620, density=49, temp=4.7, wind=3.3 ],
  [ veh_speed=9,  flow=625, density=50, temp=4.6, wind=3.4 ]
]

y = [ emission_index_next_gco2_per_km = 235 ]
```

### Example B – free-flow (Labdarių g.)

```
X =
[
  [ veh_speed=36, flow=240, density=12, temp=22.0, wind=2.0 ],
  [ veh_speed=35, flow=235, density=12, temp=22.2, wind=1.8 ],
  [ veh_speed=34, flow=230, density=11, temp=22.3, wind=1.9 ],
  [ veh_speed=33, flow=228, density=11, temp=22.5, wind=2.1 ]
]

y = [ emission_index_next_gco2_per_km = 150 ]
```

### Example C – winter + low friction (Gedimino pr.)

```
X =
[
  [ veh_speed=14, flow=520, density=40, friction=0.42, road_temp=-3.0 ],
  [ veh_speed=12, flow=540, density=42, friction=0.39, road_temp=-3.5 ],
  [ veh_speed=11, flow=555, density=44, friction=0.37, road_temp=-3.8 ],
  [ veh_speed=10, flow=565, density=46, friction=0.35, road_temp=-4.0 ]
]

y = [ emission_index_next_gco2_per_km = 222 ]
```

---

## 📝 Notes

* **X:** multivariate sequence of traffic, weather, road-surface and time-context features for past *n* steps
* **y:** selected next-step target(s); models may be trained for single- or multi-task prediction
* Raw API / SUMO XML → cleaned → scaled
* Provide your own **HERE / OpenWeather API keys** before running the pipeline
* SUMO scenario outputs support testing & what-if analysis

---
