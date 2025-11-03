# /prompts — Vilnius Traffic Congestion Predictor (GAI Agent)

This folder contains prompt assets for a **single AI agent** that orchestrates the end‑to‑end pipeline:
data acquisition → preprocessing/translation/quality checks → feature window validation → ML inference (LSTM/BiLSTM/CNN‑LSTM/Transformer) → reasoning & explanation → KPI output (jam_factor_next, travel_time_next, emission_index_next).

Files:
- `agent_instructions.md` — canonical system prompt used to bind the agent to your thesis pipeline.
- `prompt1_zero_shot.md` — simple, zero‑shot example for one road segment/time step.
- `prompt2_few_shot.md` — few‑shot example reusing (X,y) pairs from Lab 1.2 to steer the agent.
- PlantUML diagrams (in `/mnt/data/uml`) visualize how the GAI agent fits into the product.

**Targets (all scaled 0–1 except where noted):**
- `jam_factor_next` (0–10), `travel_time_next_min_per_km` (min/km), `emission_index_next_gco2_per_km` (g CO₂/km).

**Notes**
- The agent treats HERE / OpenWeather / Eismoinfo fields as authoritative and standardizes multilingual text → English.
- Numeric features are min‑max scaled to [0,1] using training stats; raw values are accepted and the agent applies the stored scaler before inference.
