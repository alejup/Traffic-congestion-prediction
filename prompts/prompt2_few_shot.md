# Prompt 2 — Few-shot with known (X,y) pairs → y
## Role: user
Use the examples to infer mapping X→y and then predict for the new case.

### Few-shot examples (from Lab 1.2)
**Example 1 — Rush hour (Gedimino pr.)**
<pre>
X = [
  { "jam_factor": 3.2, "veh_speed": 24, "temp": 14.0, "wind": 3.1, "friction": 0.87 },
  { "jam_factor": 4.1, "veh_speed": 21, "temp": 13.8, "wind": 3.3, "friction": 0.86 },
  { "jam_factor": 4.8, "veh_speed": 17, "temp": 13.5, "wind": 3.2, "friction": 0.83 },
  { "jam_factor": 5.6, "veh_speed": 12, "temp": 13.0, "wind": 3.6, "friction": 0.80 }
]
y = { "jam_factor_next": 6.3 }
</pre>pre>

**Example 2 — Weather deterioration (Vilniaus g.)**
<pre>
X = [
  { "jam_factor": 2.0, "veh_speed": 32, "temp": 25.0, "wind": 2.5, "precipitation": 0.0 },
  { "jam_factor": 2.8, "veh_speed": 28, "temp": 24.5, "wind": 3.2, "precipitation": 0.0 },
  { "jam_factor": 3.5, "veh_speed": 22, "temp": 24.0, "wind": 5.5, "precipitation": 1.2 },
  { "jam_factor": 4.7, "veh_speed": 18, "temp": 23.2, "wind": 6.1, "precipitation": 2.8 }
]
y = { "jam_factor_next": 5.5 }
</pre>

**Example 3 — Icy surface (V. Kudirkos a.)**
<pre>
X = [
  { "jam_factor": 3.9, "veh_speed": 20, "friction": 0.74, "road_temp": 0.5 },
  { "jam_factor": 4.4, "veh_speed": 18, "friction": 0.72, "road_temp": 0.2 },
  { "jam_factor": 5.0, "veh_speed": 15, "friction": 0.70, "road_temp": -0.5 },
  { "jam_factor": 5.5, "veh_speed": 12, "friction": 0.67, "road_temp": -1.0 }
]
y = { "jam_factor_next": 6.2 }
</pre>

### New prediction request
<pre>
**segment_id:** "vilnius.vilniaus_g.014"
**timestamp_utc:** "2025-11-03T16:00:00Z"
**window:**
[
  { "jam_factor": 4.8, "veh_speed": 16, "precipitation": 3.2, "wind": 5.4 },
  { "jam_factor": 5.6, "veh_speed": 12, "precipitation": 4.0, "wind": 6.1 },
  { "jam_factor": 6.1, "veh_speed": 10, "precipitation": 4.7, "wind": 6.3 },
  { "jam_factor": 6.5, "veh_speed": 9,  "precipitation": 5.0, "wind": 6.5 }
]
**target:** "travel_time_next_min_per_km"
</pre>

**Return** the JSON object defined in the *Output contract*.


#GPT5 Respose

<pre>
{
  "segment_id": "vilnius.vilniaus_g.014",
  "timestamp_utc_input": "2025-11-03T16:00:00Z",
  "timestamp_utc_predicted": "2025-11-03T16:15:00Z",
  "travel_time_next_min_per_km": 7.4,
  "model_notes": {
    "inferred_mapping_basis": "From examples: rising jam + worsening weather → higher next-step congestion metric; here jam rises to 6.5 with heavy precipitation and speed drop",
    "logic": "Current ~60/9 ≈ 6.7 min/km; trend = slowing speeds (16→12→10→9) → projected ~8 km/h → ~7.5 min/km",
    "uncertainty": "medium",
    "context": "Severe rain + steady speed decline suggests continued slowdown"
  }
}
</pre>

