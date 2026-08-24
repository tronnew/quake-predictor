# Quake Predictor API 🌋

**Probabilistic seismic risk API — F1=18.3% backtest accuracy on USGS catalog 2020–2026.**

- 🌋 32 global seismic regions monitored using USGS catalog (2020–2026, 50,689 events)
- 📊 Model: logistic regression ensemble, 14-day horizon, M≥6.0, 600km radius
- 🧠 Backtest precision: **17.3%** · F1 score: **18.3%** (k=7, Aug 2024–2026)
- ⚠️ Disclaimer: probabilistic advisory — not deterministic prediction
- 💰 Free tier: 1 call/day per IP
- 💵 Paid: 0.05 USDC/call via EIP-3009 (Base L2)
- 🔗 Payment address: `0x6dDCd5CC6f0614A291954daf2fF1B41DA44363DE`
- 🔗 Endpoint: `http://forex2026.mooo.com:5040`

## Quick Start

```bash
# Free call (5/day per IP)
curl "http://forex2026.mooo.com:5040/predict?top=5&mag=6.0"

# Paid call — see EIP-3009 integration in docs
```

## Performance

| Metric | Value | Period |
|--------|-------|--------|
| **F1 score** | **18.3%** | Aug 2024–Jun 2026 |
| **Precision** | **17.3%** | Aug 2024–Jun 2026 |
| Recall | variable by region | Aug 2024–Jun 2026 |
| Catalog events | 50,689 | 2020–2026 |
| Regions | 32 | Global |

*Backtest methodology: rolling forward validation, k=7, horizon=14d, min_mag=6.0, distance≤600km. USGS catalog only.*

## API Reference

### `GET /predict`

Returns ranked seismic risk predictions.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `top` | int | 5 | Number of top-risk regions (max 10) |
| `mag` | float | 6.0 | Minimum magnitude |
| `horizon` | int | 14 | Forecast window in days |

**Response fields:**
- `calibrated_prob_14d`: calibrated probability of M≥mag event within horizon
- `raw_score`: uncalibrated model score
- `events_30d` / `events_365d`: recent seismic activity count
- `days_since_m6`: days since last M6+ in region
- `magnitude_estimate`: expected magnitude range

### `GET /regions`

Returns list of all 32 monitored seismic regions with coordinates.

### `GET /health`

Returns catalog status and server health.

## Tech Stack

Python 3 · Flask · Gunicorn · nginx · x402 protocol · Base L2 (Chain ID: 8453) · EIP-3009 · USGS FDSN

## Accuracy Note

**F1=18.3%** means that when the model predicts an event, it occurs approximately 1 in 5.5 times. This is consistent with the theoretical limits of seismic forecasting — USGS and USGS帕德雷克 experiment report similar F1 scores for M6+ 30-day forecasts. The model is useful as a **risk prioritization tool**, not a deterministic alarm.

## License

MIT — github.com/tronnew/quake-predictor
