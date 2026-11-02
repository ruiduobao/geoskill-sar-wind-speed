# SAR Sea Surface Wind Retrieval (geoskill-sar-wind-speed)

> SAR sea surface wind field retrieval: numerical inversion of the sea surface wind speed field from the backscatter coefficient σ⁰ and wind direction via bisection, based on simplified CMOD5/CMOD7 empirical geophysical models.

---

## 1. Overview

Retrieves the sea surface wind speed at 10 m height from the SAR backscatter coefficient σ⁰, using the simplified CMOD (C-band Model) empirical model: σ⁰_dB(U, φ, θ) = [a0 + a1·θ] + [s0 + s1·θ]·U·M(φ) M(φ) = 1 + m1·cosφ + m2·cos2φ where U is the wind speed (m/s), θ is the incidence angle, φ is the angle between the wind direction and the radar line of sight, and M(φ) is the azimuth modulation (upwind > crosswind > downwind). Since σ⁰_dB increases monotonically with U, the inversion solves for the root per pixel using a vectorized bisection method, supporting both CMOD5 and CMOD7 empirical coefficient sets.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-sar-wind-speed.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `wind_speed.tif` | GeoTIFF | Retrieved wind speed field (m/s) |
| `retrieval_params.json` | JSON | Model coefficients, wind direction, incidence angle and other parameters |
| `output-manifest.json` | JSON | Run manifest (incl. synthetic-mode RMSE/correlation QA) |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
