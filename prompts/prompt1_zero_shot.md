# Prompt 1 — Zero-shot single observation → y
## Role: user
Predict the **next 15‑min jam factor** for the following Vilnius segment using the end‑to‑end pipeline.

**segment_id:** "vilnius.gedimino_pr.001"
**timestamp_utc:** "2025-11-03T07:45:00Z"

**window (oldest→newest, last 4 steps):**
[
  { "jam_factor": 3.2, "veh_speed": 24, "temp": 14.0, "wind": 3.1, "friction": 0.87 },
  { "jam_factor": 4.1, "veh_speed": 21, "temp": 13.8, "wind": 3.3, "friction": 0.86 },
  { "jam_factor": 4.8, "veh_speed": 17, "temp": 13.5, "wind": 3.2, "friction": 0.83 },
  { "jam_factor": 5.6, "veh_speed": 12, "temp": 13.0, "wind": 3.6, "friction": 0.80 }
]

**target:** "jam_factor_next"

**Return** the JSON object defined in the *Output contract*.


#GPT5 Response

{
  "segment_id": "vilnius.gedimino_pr.001",
  "timestamp_utc_input": "2025-11-03T07:45:00Z",
  "timestamp_utc_predicted": "2025-11-03T08:00:00Z",
  "jam_factor_next": 6.5,
  "model_notes": {
    "trend_basis": "recent jam_factor increases (+0.9, +0.7, +0.8) with falling speed and decreasing friction",
    "confidence": "medium",
    "reasoning": "continued congestion trend with deteriorating friction and veh_speed"
  }
}
